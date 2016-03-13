---
layout: post
title: "Florence & Edison, 2014 Landsat"
tagline: clip, label and gif
tags:
  - landsat
  - drought
  - california
---

![](http://i.imgur.com/aXJiAfr.gif)

Continuing upon [my last post](http://joelarson.com/landsat/2014/12/07/landsat-animation/) using landsat-util and animating Landsat scenes - I took a look at path 42 row 34, where [Florence and Lake Thomas Edison](https://www.openstreetmap.org/#map=12/37.3249/-118.9845) lie in the Sierra Nevadas of Central California.

I browsed [EarthExplorer](http://earthexplorer.usgs.gov/) and copied the scenes I wanted.

path 42 row 34  
LC80420342014015LGN00 01-15-14  
LC80420342014047LGN00 02-16-14  
LC80420342014079LGN00 03-20-14  
LC80420342014095LGN00 04-05-14  
LC80420342014143LGN00 05-23-14  
LC80420342014175LGN00 06-24-14  
LC80420342014207LGN00 07-26-14  
LC80420342014239LGN00 08-27-14  
LC80420342014255LGN00 09-12-14  
LC80420342014287LGN00 10-14-14  
LC80420342014319LGN00 11-15-14  
LC80420342014335LGN00 12-01-14  

To download all scenes at once, with landsat-util already installed - run:

    landsat download LC80420342014015LGN00 && landsat download LC80420342014047LGN00 && landsat download LC80420342014079LGN00 && landsat download LC80420342014095LGN00 && landsat download LC80420342014143LGN00 && landsat download LC80420342014175LGN00 && landsat download LC80420342014207LGN00 && landsat download LC80420342014239LGN00 && landsat download LC80420342014255LGN00 && landsat download LC80420342014287LGN00 && landsat download LC80420342014319LGN00 && landsat download LC80420342014335LGN00 

Process them all, takes a while - even longer if you pan-sharpen:

    landsat process ~/landsat/zip/LC80420342014015LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014047LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014079LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014095LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014143LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014175LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014207LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014239LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014255LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014287LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014319LGN00.tar.bz && landsat process ~/landsat/zip/LC80420342014335LGN00.tar.bz
    
You could make those bash scripts and run both like `./download.sh && ./process.sh`.

Next is the clip, label and gif combo. I run these from the `processed` folder that landsat-util created.

###clip.sh
	#!/bin/bash
	mkdir clip
	for i in $(find . -name "*.TIF" -type f)
	do 
	  dir="$(dirname ${i#./})"
	  gdal_translate -projwin -13264761 4497519 -13225720 4470697 -of GTiff $i clip/$dir"-clip.tif"
	done

Of course you'll want your own clip area. BYO bbox. Bring your own projwin. Maybe you'll use QGIS like me, if so - be sure to swap the `uly` & `lry` coordinates. ![](http://i.imgur.com/rF2tnc1.gif) 

###label.sh
    #!/bin/bash
	mkdir gif
	mogrify -format gif clip/*.tif
	convert clip/LC80420342014015LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'January' gif/01jan.gif
	convert clip/LC80420342014047LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'February' gif/02feb.gif
	convert clip/LC80420342014079LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'March' gif/03mar.gif
	convert clip/LC80420342014095LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'April' gif/04apr.gif
	convert clip/LC80420342014143LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'May' gif/05may.gif
	convert clip/LC80420342014175LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'June' gif/06jun.gif
	convert clip/LC80420342014207LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'July' gif/07jul.gif
	convert clip/LC80420342014239LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'August' gif/08aug.gif
	convert clip/LC80420342014255LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'September' gif/09sep.gif
	convert clip/LC80420342014287LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'October' gif/10oct.gif
	convert clip/LC80420342014319LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'November' gif/11nov.gif
	convert clip/LC80420342014335LGN00-clip.gif -fill rgba\(255,255,255,0.6\) -gravity NorthWest -pointsize 65 -annotate +5+5 'December' gif/12dec.gif

###gif.sh
    #!/bin/bash
	gifsicle gif/*.gif > anim.gif
	gifsicle --loop=0 --delay=150 --colors 256 gif/*.gif > anim.gif

All together now `./clip.sh && ./label.sh && ./gif.sh`. Yes, this could be more efficient but who knows your skill level...you may even want to [build your scripts in a spreadsheet](http://peakgis.com/2013/10/02/batch-clipping-multiple-rasters-in-qgis-a-very-basic-approach-to-a-repetitive-process/).

**Bonus**: Lake Oroville, California (2014) pansharpened [http://i.imgur.com/biRxZqG.gif](http://i.imgur.com/biRxZqG.gif)

