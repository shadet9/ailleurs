---
title: Convert KML by cli
author: Ariel
date: "2022-05-17"
---

While I must admit that Ovitalmap is one of the best field survey app in China, it's still pain in the ass when you export the geodata you made just after a long day survey trip. It turns out you can only export ".ovkml-like" format if you have no subscription. And that's okay with me, you can change the suffix back, because it's a kml file with a Ovi name😂. What annoys me most is it transforms coordinate system to GCJ02.

## Finding the tool

There are many ways to transform GCJ02 to WGS84. You can find a lot of python scripts on github, and most of them are dedicated to csv, which in this case is useless to me. Because I want to find the fastest way to do it instead of writing codes. Fortunately, I did find something on github, it's a [command line tool](https://github.com/wandergis/coordtransform-cli) which can change geojson coordinate system to/from many Chinese special coordinate systems.

## kml to geojson

First, convert kml to geojson. If you already installed GDAL, you can just use it to convert. But it won't keep style after converting, so better way to do it is to install this [cli](https://github.com/mapbox/togeojson) made by mapbox and it does convert the style into the geojson.(If you haven't installed Nodejs, you should do it before type any command below.)

install cli tool.

```bash
npm install -g @mapbox/togeojson
```

Convert kml to geojson

```bash
togeojson file.kml > file.geojson
```

## GCJ02 to WGS84

install coordinate transform tool

```bash
npm install -g coordtransform-cli
```

convert GCJ02 to WGS84

```bash
coordtransform -t gcj02towgs84  file.geojson output.geojson
```

## geojson to kml

If geojson works fine for you, then you don't have to change it back.If you still need to use kml, then continue reading👀.

At first I tried to use [tokml](https://github.com/mapbox/tokml)(also made by mapbox), but the result was not satisfying. It changed feature's name to default, and embedded the original name to somewhere else. So I tried using [ogr2ogr](https://gdal.org/programs/ogr2ogr.html), and it worked perfectly.

First install GDAL(Presumed you installed conda already)

```bash
conda install -c conda-forge gdal
```

convert kml to geojson

```bash
ogr2ogr -f KML output.kml output.geojson
```

So, actually it's just a **three-rows** command lines solution for this problem.
But I did spend some hours figuring it out, I won't say it was waste of time, but I should absolutely be more effective.

---

It's been a while(**more than two years!**) since I've written anything. Also it's my first English post!!!🤯
