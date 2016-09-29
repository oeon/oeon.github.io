---
layout: post
title: "Landsat animation"
tagline: shrinking dragon
category: landsat
tags:
  - landsat
  - drought
  - california
style: |
  body {
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  }
---

2014 [Lake Nacimiento](http://www.openstreetmap.org/#map=12/35.7432/-120.9483) in San Luis Obispo County - California

![Lake Nacimiento animation](http://i.imgur.com/s7ukngS.gif)

Inspired by: [https://twitter.com/developmentseed/status/539831619000217600](https://twitter.com/developmentseed/status/539831619000217600) I wanted to play with some Landsat data to see if I could do a time series animation for 2014 to see if Central California's recent drought could be effectively visualized. You can see some subtle shrinking of the '[Dragon Lake](http://en.wikipedia.org/wiki/Lake_Nacimiento)' ...which is not as dramatic as Folsom or Shasta reservoirs, but still apparent.

It took a bit of time downloading & processing (_pan-sharpening took about 50-60 minutes on my MacBook pro_) and space (10GB+ for 11 scenes). My process could definitely be more scripted/streamlined. But I had fun and learned a few things :)

### using landsat-util
[https://github.com/developmentseed/landsat-util](https://github.com/developmentseed/landsat-util)  

	landsat process --pansharpen LC80430352014022LGN00.tar.gz

### clip
**rename first, e.g. 4310final-pan.TIF**  

	#!/bin/bash
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014310LGN00/4310final-pan.TIF ~/landsat/processed/4310final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014294LGN00/4294final-pan.TIF ~/landsat/processed/4294final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014262LGN00/4262final-pan.TIF ~/landsat/processed/4262final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014214LGN00/4214final-pan.TIF ~/landsat/processed/4214final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014198LGN00/4198final-pan.TIF ~/landsat/processed/4198final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014166LGN00/4166final-pan.TIF ~/landsat/processed/4166final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014134LGN00/4134final-pan.TIF ~/landsat/processed/4134final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014118LGN00/4118final-pan.TIF ~/landsat/processed/4118final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014070LGN00/4070final-pan.TIF ~/landsat/processed/4070final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014054LGN00/4054final-pan.TIF ~/landsat/processed/4054final-pan-clip.TIF
	gdal_translate -projwin -13475810.0 4270507.0 -13455685.0 4256681.0 -of GTiff ~/landsat/processed/LC80430352014022LGN00/4022final-pan.TIF ~/landsat/processed/4022final-pan-clip.TIF

### convert
	mogrify -format gif *.TIF

### label
	#!/bin/bash
	convert 4022final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'January 22' gifify/01jan.gif
	convert 4054final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'February 23' gifify/02feb.gif
	convert 4070final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'March 11' gifify/03mar.gif
	convert 4118final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'April 28' gifify/04apr.gif
	convert 4134final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'May 14' gifify/05may.gif
	convert 4166final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'June 15' gifify/06jun.gif
	convert 4198final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'July 17' gifify/07jul.gif
	convert 4214final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'August 02' gifify/08aug.gif
	convert 4262final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'September 19' gifify/09sep.gif
	convert 4294final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'October 21' gifify/10oct.gif
	convert 4310final-pan-clip.gif -fill rgba\(255,255,255,0.5\) -gravity North -pointsize 65 -annotate +0+5 'November 06' gifify/11nov.gif

### gif-ify
	#!/bin/bash
	gifsicle *.gif > anim.gif
	gifsicle --loop=0 --delay=150 --colors 256 *.gif > anim.gif

# fin

**notes**  

path 43 row 35  
LC80430352014022LGN00 22-JAN-14  
LC80430352014054LGN00 23-FEB-14  
LC80430352014070LGN00 11-MAR-14  
LC80430352014118LGN00 28-APR-14  
LC80430352014134LGN00 14-MAY-14  
LC80430352014166LGN00 15-JUN-14  
LC80430352014198LGN00 17-JUL-14  
LC80430352014214LGN00 02-AUG-14  
LC80430352014262LGN00 19-SEP-14  
LC80430352014294LGN00 21-OCT-14  
LC80430352014310LGN00 06-NOV-14  
