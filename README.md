Introduction
-------------------
This dataset contains 125,192,184 computer generated building footprints in all 50 US states. This data is freely available for download and use.

License
-------------------
This data is licensed by Microsoft under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/)

## FAQ
#### What the data include:
125,192,184 building footprint polygon geometries in all 50 US States in GeoJSON format.

#### What is the GeoJson format?
GeoJSON is a format for encoding a variety of geographic data structures. 
For Intensive Documentation and Tutorials, Refer to [GeoJson Blog](http://geojson.org/)

#### Creation Details:
The building extraction is done in two stages:
1.	Semantic Segmentation – Recognizing building pixels on the aerial image using DNNs
2.	Polygonization – Converting building pixel blobs into polygons
### Semantic Segmentation
![](/images/segmentation.PNG)


#### DNN architecture
The network foundation is ResNet34 which can be found [here](https://github.com/Microsoft/CNTK/blob/master/PretrainedModels/Image.md#resnet). In order to produce pixel prediction output, we have appended RefineNet upsampling layers described in this [paper](https://arxiv.org/abs/1611.06612).
The model is fully-convolutional, meaning that the model can be applied to an image of any size (constrained by GPU memory, 4096x4096 in our case).

#### Training details
The training set consists of 5 million labeled images. Majority of the satellite images cover diverse residential areas in the US. For the sake of good set representation, we have enriched the set with samples from various areas covering mountains, glaciers, forests, deserts, beaches, coasts, etc.
Images in the set are of 256x256 pixel size with 1 ft/pixel resolution.
The training is done with CNTK toolkit using 32 GPUs.

#### Metrics
These are the intermediate stage metrics we use to track DNN model improvements and they are pixel based.
The pixel error on the evaluation set is 1.15%.
Pixel recall/precision = 94.5%/94.5%

### Polygonization
![](/images/polygonization.PNG)

#### Method description
We developed a method that approximates the prediction pixels into polygons making decisions based on the whole prediction feature space. This is very different from standard approaches, e.g. the Douglas-Peucker algorithm, which are greedy in nature. The method tries to impose some of a priori building properties, which is, at the moment, manually defined and automatically tuned. Some of these a priori properties are:
1. The building edge must be of at least some length, both relative and absolute, e.g. 3 meters
2. Consecutive edge angles are likely to be 90 degrees
3. Consecutive angles cannot be very sharp, smaller by some auto-tuned threshold, e.g. 30 degrees
4. Building angles likely have very few dominant angles, meaning all building edges are forming an angle of (dominant angle _&pm;_ n&pi;/2)

In near future, we will be looking to deduce this automatically from existing building information.

#### Metrics
Building matching metrics:

| Metric | Value |
| --- | :---: |
| Precision | 99.3% |
| Recall | 93.5% |

We track various metrics to measure the quality of the output:
1. Intersection over Union – This is the standard metric measuring the overlap quality against the labels
2. Shape distance – With this metric we measure the polygon outline similarity
3. Dominant angle rotation error – This measures the polygon rotation deviation

![](/images/bldgmetrics.JPG)

On our evaluation set contains ~15k building. The metrics on the set are:
- IoU is 0.85, Shape distance is 0.33, Average rotation error is 1.6 degrees
- The metrics are better or similar compared to OSM building metrics against the labels

#### Data Vintage
The vintage of the footprints depends on the vintage of the underlying imagery. Because Bing Imagery is a composite of multiple sources it is difficult to know the exact dates for individual pieces of data.

#### How good are the data?
Our metrics show that in the vast majority of cases the quality is at least as good as data hand digitized buildings in OpenStreetMap. It is not perfect, particularly in dense urban areas but it is still awesome.

### What is the coordinate reference system?
EPSG: 4326

#### Will Microsoft be open sourcing the models?
Yes. We are working through the internal process to open source the segmentation models and polygonization algorithms.

#### Will there be more data coming for other geographies?
Maybe. This is a work in progress.

#### Why is the data being released?
Microsoft has a continued interest in supporting a thriving OpenStreetMap ecosystem.

#### Should we import the data into OpenStreetMap?
Maybe. Never overwrite the hard work of other contributors or blindly import data into OSM without first checking the local quality. While our metrics show that this data meets or exceeds the quality of hand-drawn building footprints, the data does vary in quality from place to place, between rural and urban, mountains and plains, and so on. Inspect quality locally and discuss an import plan with the community. Always follow the [OSM import community guidelines](https://wiki.openstreetmap.org/wiki/Import/Guidelines).

### External References
The building data are featured in a recent [NYTimes article](https://www.nytimes.com/interactive/2018/10/12/us/map-of-every-building-in-the-united-states.html)

A Vector Tile implementation of the data is hosted by [Esri](https://www.arcgis.com/home/item.html?id=f40326b0dea54330ae39584012807126)


| State         | Number of Buildings  | Unzipped MB |
| ------------- |:-------------:| -----:|
| [Alabama](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Alabama.zip)|2,460,404|526|
| [Alaska](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Alaska.zip)|110,746|26|
| [Arizona](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Arizona.zip)|2,555,395|584|
| [Arkansas](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Arkansas.zip)|1,508,657|321|
| [California](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/California.zip)|10,988,525|2,537|
| [Colorado](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Colorado.zip)|2,080,808|466|
| [Connecticut](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Connecticut.zip)|1,190,229|258|
| [Delaware](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Delaware.zip)|345,907|75|
| [District Of Columbia](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/DistrictofColumbia.zip)|58,329|13|
| [Florida](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Florida.zip)|6,903,772|1,521|
| [Georgia](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Georgia.zip)|3,873,560|825|
| [Hawaii](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Hawaii.zip)|252,891|56|
| [Idaho](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Idaho.zip)|883,594|198|
| [Illinois](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Illinois.zip)|4,855,794|1,033|
| [Indiana](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Indiana.zip)|3,268,325|700|
| [Iowa](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Iowa.zip)|2,035,688|427|
| [Kansas](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Kansas.zip)|1,596,495|339|
| [Kentucky](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Kentucky.zip)|2,384,214|500|
| [Louisiana](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Louisiana.zip)|2,057,368|448|
| [Maine](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Maine.zip)|752,054|160|
| [Maryland](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Maryland.zip)|1,622,849|344|
| [Massachusetts](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Massachusetts.zip)|2,033,018|438|
| [Michigan](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Michigan.zip)|4,900,472|1,045|
| [Minnesota](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Minnesota.zip)|2,815,784|606|
| [Mississippi](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Mississippi.zip)|1,495,864|321|
| [Missouri](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Missouri.zip)|3,141,265|662|
| [Montana](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Montana.zip)|762,288|168|
| [Nebraska](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Nebraska.zip)|1,158,081|245|
| [Nevada](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Nevada.zip)|932,025|212|
| [New Hampshire](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/NewHampshire.zip)|563,487|122|
| [New Jersey](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/NewJersey.zip)|2,480,332|528|
| [New Mexico](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/NewMexico.zip)|1,011,373|231|
| [New York](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/NewYork.zip)|4,844,438|1,035|
| [North Carolina](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/NorthCarolina.zip)|4,561,262|963|
| [North Dakota](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/NorthDakota.zip)|559,161|121|
| [Ohio](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Ohio.zip)|5,449,419|1,164|
| [Oklahoma](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Oklahoma.zip)|2,091,131|455|
| [Oregon](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Oregon.zip)|1,809,555|408|
| [Pennsylvania](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Pennsylvania.zip)|4,850,273|1,034|
| [Rhode Island](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/RhodeIsland.zip)|366,779|78|
| [South Carolina](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/SouthCarolina.zip)|2,180,513|463|
| [South Dakota](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/SouthDakota.zip)|649,737|138|
| [Tennessee](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Tennessee.zip)|3,002,503|638|
| [Texas](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Texas.zip)|9,891,540|2,141|
| [Utah](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Utah.zip)|1,004,734|226|
| [Vermont](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Vermont.zip)|345,911|75|
| [Virginia](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Virginia.zip)|3,057,019|643|
| [Washington](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Washington.zip)|2,993,361|675|
| [West Virginia](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/WestVirginia.zip)|1,020,031|213|
| [Wisconsin](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Wisconsin.zip)|3,054,452|654|
| [Wyoming](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Wyoming.zip)|380,772|83|













<br>
<br>

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all others rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
