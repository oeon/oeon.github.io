---
published: true
layout: post
category: landsat
tagline: my basic intro
tags:
  - remote sensing
  - landsat
  - landsat8
  - open source
style: |
  body {
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  }
---

**tl;dr** MapBox has some nice material on using Landsat 8 imagery. Without previous experience processing Landsat data, I used stuff learned from four of their blog posts. Here are some of [my notes](https://gist.github.com/oeon/8007457), images and [a map](http://joelarson.com/harmony-landsat/map.html) of some local, recent Landsat imagery...nothing crazy, just a little do-it-yourself red-green-blue, false-color and color-infrared.

<a href="http://www.flickr.com/photos/j03lar50n/11415201475" title="Avenal, CA"><img src="http://farm3.staticflickr.com/2871/11415201475_b84df2f585.jpg" width="500" height="500" alt=""></a>

Initial inspiration came from [Charlie's](https://www.mapbox.com/about/team/#charlie-loyd) two posts this summer - [Putting Landsat 8’s Bands to Work](https://www.mapbox.com/blog/putting-landsat-8-bands-to-work/ "Putting Landsat 8’s Bands to Work") and [Processing Landsat 8 Using Open-Source Tools](https://www.mapbox.com/blog/processing-landsat-8/ "Processing Landsat 8 Using Open-Source Tools"). Pansharpening gets you 15 meter resolution and [Chris](https://www.mapbox.com/about/team/#chris-herwig) wrote the [main steps involved](https://www.mapbox.com/blog/pansharpening-satellite-imagery-openstreetmap/) in pansharpening with Orfeo Toolbox and GDAL.

<a href="http://www.flickr.com/photos/j03lar50n/11414378274/" title="salinas-false by j03lar50n, on Flickr"><img src="http://farm4.staticflickr.com/3818/11414378274_abe403192a_c.jpg" width="800" height="493" alt="salinas-false"></a> <p align="center"><small>December 5, 2013 Salinas, CA. False-color combination of NIR, SWIR, and visible red, using bands 5, 6, and 4 + pansharpened, Landsat 8.</small></p>

I used Mac OS X and was hoping to evoke `otbcli` from Terminal command line. Thinking I could install Orfeo via Homebrew - it seems at the writing of this post there are various issues with _homebrewed_ Orfeo/Mountain Lion. `otbcli` is contained within Monteverdi toolbox and I could probably access the applications/utilities from there but I already had QGIS installed and they were available from `/Applications/QGIS.app/Contents/MacOS/bin/` on my system, so I used those.

<a href="http://www.flickr.com/photos/j03lar50n/11414215945/" title="hh-testburn and estero by j03lar50n, on Flickr"><img src="http://farm6.staticflickr.com/5491/11414215945_41bdf6afa1_c.jpg" width="800" height="500" alt="hh-testburn and estero"></a> <p align="center"><small>Upper left, Harmony Headlands prescribed burn *just* getting underway - note the strong offshore winds pushing the smoke southwest. Lower right, Estero rx-burn from November. Image date: December 5, 2013.</small></p>

Chris's post involves GeoEye imagery, the code is pretty much he same if you use Landsat except I used [this snippet](https://github.com/dwtkns/gdal-cheat-sheet/blob/master/README.md#raster-operations) from [Derek](https://github.com/dwtkns) for converting 16-bit bands (UInt16) to Byte type `gdal_translate -ot Byte -scale 0 65535 0 255 -a_nodata "0 0 0" pansharp.tif pansharp-scaled.tif ` (only parameter that's different is the `scale` value was 3000).

<a href="http://www.flickr.com/photos/j03lar50n/11413962596/" title="paso-rgb by j03lar50n, on Flickr"><img src="http://farm4.staticflickr.com/3811/11413962596_08176a44b6_b.jpg" width="800" height="600" alt="paso-rgb"></a> <p align="center"><small><em>Wine country, San Luis Obispo County. You can see hints of red as the grape vines lose their leaves.</em></small></p>

I ended up using `convert -monitor -sigmoidal-contrast 50x12% RGB.tif RGB-corrected.tif` for my regular Red-Green-Blue. And `convert -monitor -sigmoidal-contrast 33x12% pansharp_3857.tif pansharp_3857-bright.tif` for my pansharpened stuff. For those keeping count of those four MapBox posts I mentioned, the last honorable mention is from [Processing RapidEye Imagery in Minutes](https://www.mapbox.com/blog/processing-rapideye-imagery/) where I used _"GDAL one last time to bundle the adjusted image data and the geographical information into a geotiff"_. Again, jotted [my steps here](https://gist.github.com/oeon/8007457).

<a href="http://www.flickr.com/photos/j03lar50n/11414557835/" title="slo-cir"><img src="http://farm6.staticflickr.com/5528/11414557835_0df81db55d_c.jpg" width="800" height="448" alt="slo-cir"></a> <p align="center"><small>City of San Luis Obispo and Santa Margarita lake, color-infrared.</small></p>

It **does** take a bit of tweaking the contrast and brightness but I was happy enough with these quick results. Next, I'd like to recreate the [Burned Area Reflectance Classification](http://www.fs.fed.us/eng/rsac/baer/barc.html) workflow. Also, more learning about Band Math, bit depth and which tool / which task for weilding Orfeo/ImageMagick/GDAL/? would be good. Check out band combinations for Landsat 8 [here](http://landsat.usgs.gov/L8_band_combos.php).

**Note**: A wiseman (Charlie) advised a couple things - _"Always be a little careful pansharpening false color. The pan band only covers the RGB range, so to the extent that the other bands don’t positively correlate with it, it’s theoretically possible for it to distort their values."_ And also, ImageMagick's `-combine` flag doesn't work correctly without a `-colorspace RGB` flag accompanying it (for some users) .. so heads-up!

Thank you kind internet people who put material out there for people to follow. Without much experience using remote sensing imagery or tools and this is a great way to get your feet wet. Again, [the map](http://joelarson.com/harmony-landsat/map.html).
