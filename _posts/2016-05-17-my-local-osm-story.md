---
layout: post
title: "My Local OpenStreetMap Story"
tagline: feed them for a lifetime
category: openstreetmap
tags:
  - openstreetmap
style: |
  body {
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  }
---

**tl;dr** I worked with interns to make a lot of edits to OpenStreetMap at my previous job - a County/State Government. OSM wins, the Organization wins, the interns win - there are tons of benefits for many. **Takeaway**: teach others to edit OSM.

## feed them for a lifetime

On a couple occasions [Coleman](http://www.colemanm.org/) has encouraged me to write about our accomplishments at my former job, where we leveraged **power in numbers**, to ferociously edit OpenStreetMap (OSM) in our local area. Aaand he's right, I should! I'm going to jot this down before he reminds me again 😜.

My background in OSM began around the time I '_cyberstalked_' [@wonderchook](https://twitter.com/wonderchook) on 6 May 2010 (thank you Twitter DM's) somewhere near Monterey, California. I sat down with her for a beer and walked away inspired...can't recall much that we talked about but I know it was enough of a catalyst to begin getting into OSM.

I live in a rural county in California, [San Luis Obispo](https://en.wikipedia.org/wiki/San_Luis_Obispo_County,_California), affectionately known as **SLO**. It is significant in size (area) and has only a handful of population centers - the largest city is less than 50,000 people and the entire county has less than 300,000. I used to work for a _full service all risk fire department_ - [CAL FIRE](https://en.wikipedia.org/wiki/California_Department_of_Forestry_and_Fire_Protection), as a GIS Analyst. Also in SLO is [Cal Poly State University](https://en.wikipedia.org/wiki/California_Polytechnic_State_University), the reason I came to the area - as well as why many other students do - which we hire(d) as interns for CAL FIRE San Luis. Being a 'rural' area and working for an emergency response organization means that I/we needed to provide good data for a quick response to people that need help, sometimes in far away and/or obscure areas.

One of the things you do as a GIS Analyst is create data, specifically geographic databases/datasets. Something that commonly happens with GIS Analysts is that the data they create either becomes [silo-ed](https://en.wikipedia.org/wiki/Information_silo), gets stale, or even is lost/destroyed - somehow, someway. During my first few years in that role, I experienced all of those possibilities. I learned that OSM could be a more permanent place for _some_ of the data we were creating in our GIS lab. I was lucky enough to have an **awesome** supervisor who let me try things that were outside-of-the-box ideas and not traditional in our line of work - and we began working on OSM.

## getting hungry

I guess you could say it all began with our lab wanting to collaboratively edit GIS data. We originally considered: pgVersion and zigGIS - from what I can recall. You may have guessed, our shop didn't have a robust budget to afford ArcGIS for Server. As a State/County government agency, we were provided a few licenses of ArcMap but not enough for our entire team (which ranged from 5-10+ interns at a time) so we used QGIS and other [FOSS4G](https://wiki.osgeo.org/wiki/FOSS4G) software to fill in the gaps. But enabling collaborative editing of data in your own office is one thing - but what about opening up contributions from others outside of your office, organization, or even your country? BTW, those aforementioned approaches never really panned out for collaborative editing, until...

**Enter OpenStreetMap**: By contributing to OSM, our lab could concurrently edit the database while also benefitting from the crowd-sourced efforts of others around the world. Data can be created, manipulated, and extracted. The map and data _we_ edit, in turn, are also used by dozens of significant companies and projects. We had  found an ideal workflow for our lab to edit simultaneously, with a low barrier to entry (cost), which also benefitted from the power of crowd-sourcing, and the ability to get the data back out!

## we feast

![OSM edits from 2009 until now](/assets/img/slo-osm-analytics.gif) [Source](http://osm-analytics.org/#/compare/polygon:noh%60V_%7DatEo%7DjGu%7DBvyzCgohDlmfC%7BSbmvAt%7D%40/2009...now/buildings)

Now with an idea on how we would proceed, we turned the interns loose. Our first project: building footprints. This project would provide multiple benefits to various users/cooperators.

1. The OpenStreetMap project would benefit by us enriching the map.
2. Our job in the GIS lab was to support the firefighters of CAL FIRE/San Luis Obispo County Fire; providing them with increased [Situational Awareness](https://en.wikipedia.org/wiki/Situation_awareness) during fire, medical, or rescue responses by having detailed maps.
3. The data we create is immediately viewable on the [https://openstreetmap.org](https://openstreetmap.org) map.
4. The data becomes available to the variety of popular services that incorporate OSM data. _For example_: [MAPS.ME](https://maps.me) was very popular with our field users because of its offline & routing capabilities.
5. The data can be extracted from OSM and converted into various formats for downstream uses/needs. _For example_: converting and sharing the data in Esri's File Geodatabase with our local County cooperators.

We were blessed by the work of the [Humanitarian OpenStreetMap Team](https://hotosm.org/) for giving us the [OSM Tasking Manager](https://github.com/hotosm/osm-tasking-manager2). This allowed us to work together in an organized and systematic way while adding our 150,000+ features across 3,300 square miles. It was also a [good exercise](http://joelarson.com/openstreetmap/2015/10/13/OSM-Task-Manager.html) for me in learning Ubuntu/Linux and Amazon EC2.

On a daily basis, this is how the lab would typically operate:

* Wire up [WhoDidIt](http://wiki.openstreetmap.org/wiki/Quality_assurance#WhoDidIt) to our Chrome [RSS Feed Reader](https://chrome.google.com/webstore/detail/rss-feed-reader/pnjaodmkngahhkoihejjehlcdlnohgmp?hl=en) for quality control of the data in our area.
* Review questionable edits with [OSM History Viewer](http://wiki.openstreetmap.org/wiki/OSM_History_Viewer) and/or [osm deep history](https://github.com/osmlab/osm-deep-history).
* Field verify with GPS units and/then eventually [Mapillary](http://www.mapillary.com/).
* Utilize the power-editing tools in the JOSM editor. And use JOSM because it rhymes with Jaw-some, go [Street Sharks](https://www.youtube.com/watch?v=NqGQyMF5a_0)!

![A ferocious Street Shark](/assets/img/street-sharks.jpg)

## digestifs

Sometimes I get a little sad 🙁 when I look at my [How did you contribute to OpenStreetMap?](http://hdyc.neis-one.org/?j03lar50n) UserLink, knowing that I've probably peaked with the amount of time I can give to editing.

![mapping days by year for j03lar50n's OSM edits](/assets/img/mapping-days-j03lar50n.png)

But then I try to justify it by telling myself that I've taught and trained dozens of others to edit OSM and the efforts are still on-going in the lab - today! I don't know ¯\\\_(ツ)\_/¯ it is what it is. No ragrets 😉. I should probably just shut up and get back to mapping!
