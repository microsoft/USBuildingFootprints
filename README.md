## Introduction

Microsoft Maps is releasing country wide open building footprints datasets in United States. This dataset contains 129,591,852 computer generated building footprints derived using our computer vision algorithms on satellite imagery. This data is freely available for download and use.

![Update regions](/images/example.JPG)

## License

This data is licensed by Microsoft under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/).

## Data Vintage

The vintage of the footprints depends on the vintage of the underlying imagery. Bing Imagery is a composite of multiple sources with different capture dates. Each building footprint has a capture date tag associated if we were able to deduce the vintage of imagery source.

Footprints inside the highlighted [region](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/US_areas_L13.geojson) on the map are from 2019-2020. There are 73,250,745 such building footprints. This is the focal area where we rerun extraction for the latest release.

The rest of the footprints were extracted from older images, having wider range of capture dates, averaging 2012 year approximately. We have reused footprints from previous releases in this area.

![Update regions](/images/update_regions.jpg)

## FAQ

### What the data include?

129,591,852 building footprint polygon geometries divided by 50 US states and the District of Columbia in GeoJSON format.

### Why is the data being released?

Microsoft has a continued interest in supporting a thriving OpenStreetMap ecosystem.

### What is the GeoJson format?

GeoJSON is a format for encoding a variety of geographic data structures.
For Intensive Documentation and Tutorials, Refer to [GeoJson Blog](http://geojson.org/).

### Should we import the data into OpenStreetMap?

Maybe. Never overwrite the hard work of other contributors or blindly import data into OSM without first checking the local quality. While our metrics show that this data meets or exceeds the quality of hand-drawn building footprints, the data does vary in quality from place to place, between rural and urban, mountains and plains, and so on. Inspect quality locally and discuss an import plan with the community. Always follow the [OSM import community guidelines](https://wiki.openstreetmap.org/wiki/Import/Guidelines).

### Will the data be used or made available in larger OpenStreetMap ecosystem?

Yes. Currently Microsoft Open Buildings dataset is used in ml-enabler for task creation. You can try it out at [AI assisted Tasking Manager](https://tasks-assisted.hotosm.org/). The data will also be made available in Facebook [RapiD](https://mapwith.ai/rapid#background=Bing&disable_features=boundaries&map=2.00/0.0/0.0).

### What is the creation process for this data?

The building extraction is done in two stages:

1. Semantic Segmentation – Recognizing building pixels on the aerial image using DNNs
2. Polygonization – Converting building pixel blobs into polygons

#### Stage1: Semantic Segmentation

![Semantic Segmentation](/images/segmentation.jpg)

##### DNN architecture and training

The network backbone we used is EfficientNet described [here](https://arxiv.org/abs/1905.11946). Although we have millions of labels at our disposal, we found that an effective combination of supervised and unsupervised training yields the best results.

#### Stage 2: Polygonization

![Polygonization](/images/polygonization.jpg)

##### Method description

We developed a method that approximates the prediction pixels into polygons making decisions based on the whole prediction feature space. This is very different from standard approaches, e.g. the Douglas-Peucker algorithm, which are greedy in nature. The method tries to impose some of a priori building properties, which is, at the moment, manually defined and automatically tuned. Some of these a priori properties are:

### How good is the data?

Our metrics show that in the vast majority of cases the quality is at least as good as data hand digitized buildings in OpenStreetMap.

#### DNN model metrics

These are the intermediate stage metrics we use to track DNN model improvements and they are pixel based.
Pixel recall/precision = 95.5%/94.0%

#### Polygon evaluation metrics

Match metrics:

| Metric | Value |
| --- | :---: |
| Precision | 98.5% |
| Recall | 92.4% |

We evaluate following metrics to measure the quality of the output:

1. Intersection over Union – This is the standard metric measuring the overlap quality against the labels
2. Shape distance – With this metric we measure the polygon outline similarity
3. Dominant angle rotation error – This measures the polygon rotation deviation

![Building metrics](/images/bldgmetrics.JPG)

On our evaluation set contains ~15k building. The metrics on the set are:

| IoU | Shape distance | Rotation error [deg] |
| :---: | :---: | :---: |
|  0.86 | 0.4 | 2.5 |

#### False positive ratio in the corpus

We estimate <1% false positive ratio in 1000 randomly sampled buildings from the entire output corpus.

### What is the coordinate reference system?

EPSG: 4326

### Will there be more data coming for other geographies?

Maybe. This is a work in progress.

## External References

The building data are featured in [NYTimes article](https://www.nytimes.com/interactive/2018/10/12/us/map-of-every-building-in-the-united-states.html).

A Vector Tile implementation of the data is hosted by [Esri](https://www.arcgis.com/home/item.html?id=f40326b0dea54330ae39584012807126).

## Download links

| State or district | Number of Buildings  | Unzipped size |
|:----------------- |:-------------:| -----:|
| [Alabama](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Alabama.geojson.zip)|2,455,168|672.58 MiB|
| [Alaska](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Alaska.geojson.zip)|111,042|30.00 MiB|
| [Arizona](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Arizona.geojson.zip)|2,738,732|806.59 MiB|
| [Arkansas](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Arkansas.geojson.zip)|1,571,198|425.40 MiB|
| [California](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/California.geojson.zip)|11,542,912|3.35 GiB|
| [Colorado](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Colorado.geojson.zip)|2,185,953|619.88 MiB|
| [Connecticut](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Connecticut.geojson.zip)|1,215,624|324.20 MiB|
| [Delaware](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Delaware.geojson.zip)|357,534|94.00 MiB|
| [District of Columbia](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/DistrictofColumbia.geojson.zip)|77,851|22.52 MiB|
| [Florida](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Florida.geojson.zip)|7,263,195|2.01 GiB|
| [Georgia](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Georgia.geojson.zip)|3,981,792|1.04 GiB|
| [Hawaii](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Hawaii.geojson.zip)|252,908|64.72 MiB|
| [Idaho](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Idaho.geojson.zip)|942,132|259.43 MiB|
| [Illinois](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Illinois.geojson.zip)|5,194,010|1.35 GiB|
| [Indiana](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Indiana.geojson.zip)|3,379,648|920.20 MiB|
| [Iowa](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Iowa.geojson.zip)|2,074,904|517.95 MiB|
| [Kansas](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Kansas.geojson.zip)|1,614,406|428.38 MiB|
| [Kentucky](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Kentucky.geojson.zip)|2,447,682|663.98 MiB|
| [Louisiana](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Louisiana.geojson.zip)|2,173,567|600.69 MiB|
| [Maine](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Maine.geojson.zip)|758,999|187.84 MiB|
| [Maryland](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Maryland.geojson.zip)|1,657,199|410.84 MiB|
| [Massachusetts](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Massachusetts.geojson.zip)|2,114,602|566.87 MiB|
| [Michigan](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Michigan.geojson.zip)|4,982,783|1.24 GiB|
| [Minnesota](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Minnesota.geojson.zip)|2,914,016|762.08 MiB|
| [Mississippi](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Mississippi.geojson.zip)|1,507,496|394.08 MiB|
| [Missouri](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Missouri.geojson.zip)|3,190,076|840.28 MiB|
| [Montana](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Montana.geojson.zip)|773,199|200.45 MiB|
| [Nebraska](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Nebraska.geojson.zip)|1,187,234|302.72 MiB|
| [Nevada](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Nevada.geojson.zip)|1,006,278|296.10 MiB|
| [New Hampshire](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/NewHampshire.geojson.zip)|577,936|146.40 MiB|
| [New Jersey](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/NewJersey.geojson.zip)|2,550,308|681.55 MiB|
| [New Mexico](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/NewMexico.geojson.zip)|1,037,096|291.54 MiB|
| [New York](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/NewYork.geojson.zip)|4,972,497|1.25 GiB|
| [North Carolina](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/NorthCarolina.geojson.zip)|4,678,064|1.22 GiB|
| [North Dakota](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/NorthDakota.geojson.zip)|568,213|143.54 MiB|
| [Ohio](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Ohio.geojson.zip)|5,544,032|1.42 GiB|
| [Oklahoma](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Oklahoma.geojson.zip)|2,159,894|582.14 MiB|
| [Oregon](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Oregon.geojson.zip)|1,873,786|545.94 MiB|
| [Pennsylvania](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Pennsylvania.geojson.zip)|4,965,213|1.23 GiB|
| [Rhode Island](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/RhodeIsland.geojson.zip)|392,581|105.21 MiB|
| [South Carolina](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/SouthCarolina.geojson.zip)|2,299,671|612.67 MiB|
| [South Dakota](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/SouthDakota.geojson.zip)|661,311|166.31 MiB|
| [Tennessee](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Tennessee.geojson.zip)|3,212,306|890.22 MiB|
| [Texas](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Texas.geojson.zip)|10,678,921|2.83 GiB|
| [Utah](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Utah.geojson.zip)|1,081,586|306.98 MiB|
| [Vermont](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Vermont.geojson.zip)|351,266|87.92 MiB|
| [Virginia](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Virginia.geojson.zip)|3,079,351|797.04 MiB|
| [Washington](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Washington.geojson.zip)|3,128,258|884.38 MiB|
| [West Virginia](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/WestVirginia.geojson.zip)|1,055,625|260.33 MiB|
| [Wisconsin](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Wisconsin.geojson.zip)|3,173,347|817.06 MiB|
| [Wyoming](https://usbuildingdata.blob.core.windows.net/usbuildings-v2/Wyoming.geojson.zip)|386,518|99.32 MiB|

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Legal Notices

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all others rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
