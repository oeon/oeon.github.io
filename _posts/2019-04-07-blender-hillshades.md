---
layout: post
title: "Yet Another Blender Hillshade Tutorial"
tagline: a guide for after-hours/part-time carto hackers
category:
tags:
  -
style: |
  body {
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  }
---

**Note**: I acknowledge that this post could use more/better images and perhaps even some GIF's or a video...but I wanted to just _get it out there_. I'll try to make it better over time. Hit me up on Twitter or send me an email with any questions

## Intro

After ogling at the cartographic works of [Daniel Huffman](https://twitter.com/pinakographos), [Andy Woodruff](https://twitter.com/awoodruff), [Scott Reinhard](https://twitter.com/scottreinhard) and others - I wanted to take my stab at making one of those gorgeous Blender hillshades. I think I fell down this 🐰🕳via Andy's post "[Relief mapping in 10 easy steps!](https://www.axismaps.com/blog/2018/05/relief-in-10-steps/)"...I don't think I've even finished all 10 steps yet 😂 but it's all been a good excuse to cut my teeth learning Blender, practice cartography with QGIS, and simply use GDAL 😍

For me, the point of all of this is purely cartographic, artistic, and hobby. Plus, one decent side effect of blogging is that you have a place to dump all of your notes for reference later. Yeah, the outputs probably won't look as good as those aforementioned legends but from what I've seen so far, it certainly looks better than what I was "raised on", when I was first getting started doing GIS ... **a lot better**. So if this sounds like your jam, hopefully something noted here will help

<p style="text-align:center"><img src= "https://s3-us-west-2.amazonaws.com/joetos/blender-hillshades/hillshade-default-example.png" style="width:360px"></p>
<p style="text-align:center; font-style:italic; font-size: smaller">example: gdaldem hillshade input-dem output-hillshade</p>
<p style="text-align:center"><img src= "https://s3-us-west-2.amazonaws.com/joetos/blender-hillshades/hillshade-blender-example.png" style="width:360px"></p>
<p style="text-align:center; font-style:italic; font-size: smaller">same area: but rendered in Blender</p>

## This tutorial

Andy points to [Daniel's great series](https://somethingaboutmaps.wordpress.com/2017/11/16/creating-shaded-relief-in-blender/). But in all honesty, I got hung up on a step in _The Plane_ section - so I googled around more and found [Matt's blog](http://mattfife.com/?p=3796) and finally [Morgan's post](https://wanderingcartographer.wordpress.com/2014/11/14/shaded-relief-with-blendergis-part-1/). Morgan's notes allowed me to get some decent results by using the [BlenderGIS plugin](https://github.com/domlysz/BlenderGIS) - and I decided to start documenting the steps to be able to show others. _But_ I was able to come back and realize [that one weird trick](#that-one-weird-trick) which I was missing for Daniel's flow

## Data & Setup

USGS's [TNM](https://viewer.nationalmap.gov/basic/) is solid for accessing DEM's. Get some data to work with from here - or wherever. The [30-Meter SRTM Tile Downloader](http://dwtkns.com/srtm30m/) has some good stuff too (just register for a username/password first)

![The National Map download interface](https://s3-us-west-2.amazonaws.com/joetos/blender-hillshades/TNM_Download.png)

* `brew cask install blender 2.79b` is the current release at the time this was written. By the way, I've used a Mac for this, so some assumptions will be made
* make sure you're on Cycles renderer <span style="text-align:center"><img src= "https://s3-us-west-2.amazonaws.com/joetos/blender-hillshades/cycles.png" style="width:600px"></span> And set your profile to default to this. Daniel covers this is [Blender Basics](https://somethingaboutmaps.wordpress.com/blender-relief-tutorial-blender-basics/)
* you will probably want to have [QGIS](https://qgis.org/) around for checking DEM properties such as dimensions and pixel ranges - and to view your results. The [Lutra Installer](https://lutraconsulting.github.io/qgis-mac-packager/) has been working 👌for me & I keep an [OSGeo Homebrew](https://osgeo.github.io/homebrew-osgeo4mac/) version installed too

### <a name="that-one-weird-trick">That One Weird Trick</a>

Honestly, it took me a few days to realize this one ¯\\\_(ツ)_/¯ and at this point, I might as well point you back to [Daniel's tutorial](https://somethingaboutmaps.wordpress.com/blender-relief-tutorial-the-plane/). And while you're at it, be sure to check out the rest of [his site](https://somethingaboutmaps.wordpress.com/) and support professional cartographers and artists like he and [Scott](https://www.scottreinhardmaps.com/). These people are true professionals who make some of their living with this work - donate to them or buy from them, if you can

Find the minimum and maximum elevation values of the source DEM, -16/3527 here, then:<br>
`gdal_translate -of GTiff -ot UInt16 -scale -16 3527 0 65535 N14W092.hgt dem-scale.tif`

**Why?**

>Our final output target is going to be a 16-bit unsigned integer TIFF. This means that each pixel can hold a number between 0 and 65,535

>You’ll need to stretch your elevation range to cover the span from 0 to 65,535

[source][1]

## Combining with topographic maps

![Silver Divide topographic map](https://joetos.s3.amazonaws.com/blender-hillshades/devils-bathtub.png)

This has been my playbook for obtaining old topos from [topoView](https://ngmdb.usgs.gov/topoview/viewer/) and combining them with [DEM's from TNM](https://viewer.nationalmap.gov/basic/) (mentioned above). I keep a `notes.txt` file with the steps below in a directory with my working files for organization. Shoutout 🗣to the QGIS team for awesome features like temporary scratch layers, better editing and snapping tools, and the Shape Digitizing Toolbar

1. Get the SRS info of the topo
  * `gdalsrsinfo CA_Dunsmuir_299340_1935_125000_geo.tif`
  * ☝️will output something like: _+proj=poly +lat_0=0 +lon_0=-122.25 +x_0=0 +y_0=0 +datum=NAD27 +units=m +no_defs_ ...log this somewhere
2. create a .geojson extent around the topo in QGIS
  * add DEM first to set projection
  * then add the topo .tif
  * New Temporary Scratch Layer, geojson/polygon
  * Digitize inside topo collar - around topo content. Save as topo-extent.geojson in working folder

3. warp & clip the DEM to the extent of the topo and into its projection
  * `gdalwarp -s_srs EPSG:4269 -t_srs '+proj=poly +lat_0=0 +lon_0=-122.25 +x_0=0 +y_0=0 +datum=NAD27 +units=m +no_defs' -r bilinear -cutline topo-extent.geojson -crop_to_cutline n42w123/imgn42w123_13.img dem.tif`

4. add DEM to QGIS and get low/high elevations for next step

5. `gdal_translate -of GTiff -ot UInt16 -scale 148.194 2525.06 0 65535 dem.tif dem-scale.tif`

6. run orig topo tif through gdal_translate if you get something like  <a href="https://joetos.s3.amazonaws.com/blender-hillshades/oops.tif" target="_blank">oops.tif</a> from the original topo
  * `gdal_translate CA_Dunsmuir_299340_1935_125000_geo.tif shasta.tif`

7. get extent of topo and extend the DEM. You can do this with GDAL, but I usually use QGIS. Layer Properties > Information
  * `gdalwarp -te -25858.8718268426709983 4536063.6242974959313869 26708.5448398239932430 4598949.7909641629084945 dem-scale.tif dem-scaled.tif`

8. note the X,Y dimensions of the topo/DEM
    ```
    Width
    4967
    Height
    5942

    For Blender scale/orthographic scale
    5942/4967 = 1.196295550634186
    1.196295550634186 * 2 = 2.392591101268371
    ```
9. Follow [Daniel's steps](https://somethingaboutmaps.wordpress.com/2017/11/16/creating-shaded-relief-in-blender/)

## Extra notes

### Satellite imagery

Bart has a great post titled, "[Photo-realistic shaded relief using Blender](https://www.barthoekstra.com/blog/photo-realistic-shaded-relief-using-blender)" 👍. I like the use of the cloudless Sentinel imagery and the Python wms-downloader

### Shadows

The GIS Whisperer [tweeted](https://twitter.com/qgiswhisp/status/1104115562723065856) something neat on shadows, "[Enhancing terrain cartography with natural shadows](https://landscapearchaeology.org/2019/qgis-shadows/)" which I tested a bit too. You can experiment with adding this on top to enhance/extend the Blender shadows

[1]: https://somethingaboutmaps.wordpress.com/blender-relief-tutorial-getting-set-up/ "Blender Relief Tutorial: Getting Set Up"

### General tips and notes

Here are a few tips imma keep around for easy copy & paste. They're not very well organized and some of them ended up not being required for the final workflows...going to note them anyway, for now:

* basic [reprojection](https://www.gdal.org/gdalwarp.html). For a while I was using UTM as a common projection for the DEM's, topos, and shadow layers I was using.

`gdalwarp -s_srs EPSG:4269 -t_srs EPSG:26912 -r bilinear grdn37w111_13/hdr.adf dem-utm12n.tif`

* scale it. Here's that [one weird trick](#that-one-weird-trick), again

`gdal_translate -of GTiff -ot UInt16 -scale 1300.43 2465.47 0 65535 dem-utm12n.tif dem-utm12n-scale.tif`

* Using an extent polygon for trimming warp artifacts (black collar/no data/etc)

`gdalwarp -cutline topo-extent.geojson -crop_to_cutline dem-utm12n-scale.tif dem-utm12n-crop.tif`

* export a worldfile for a dataset with known geobounds to use with a blender render which won't have any

```
gdalwarp -co "TFW=YES" las-cruces-blender-dem.tif deleteme.tif
rm deleteme.tif
mv deleteme.tfw las-cruces-blender-hillshade.tfw
```

* crop the hillshade. Say you want to digitize (QGIS) or otherwise produce (GDAL) an extent polygon to use for clipping your Blender hillshade. This is that same file from above with a companion .tfw, so it'll produce a file with easy georeferenceability for carto hacking in QGIS

`gdalwarp -s_srs EPSG:26913 -t_srs EPSG:26913 -cutline extent-topo.geojson -crop_to_cutline las-cruces-hillshade.tif las-cruces-hillshade-crop.tif`

* when reprojecting, `-r bilinear` resampling has worked best for me so far. `-tr 5 5` if you need to scale the cell size/resolution

`gdalwarp -s_srs EPSG:4269 -t_srs EPSG:26913 -r bilinear -tr 5 5 -cutline extent-shadow.geojson -crop_to_cutline -of GTiff USGS_NED_13_n33w107_IMG.img las-cruces-shadow-dem.tif`

* convert NAD27 Polyconic to UTM. Some of the old topos were in the projection. Also, `-dstnodata 0 -dstalpha` is handy if you want to blank out black collars/no data artifacts from warping

`gdalwarp -t_srs EPSG:26912 -r bilinear -dstnodata 0 -dstalpha AZ_Marsh\ Pass_315539_1883_250000_geo.tif marshpass-utm12n.tif`