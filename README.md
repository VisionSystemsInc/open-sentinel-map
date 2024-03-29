## OpenSentinelMap

The OpenSentinelMap dataset contains Sentinel-2 imagery and per-pixel semantic label masks derived from OpenStreetMap. It is described in [this paper](https://openaccess.thecvf.com/content/CVPR2022W/EarthVision/papers/Johnson_OpenSentinelMap_A_Large-Scale_Land_Use_Dataset_Using_OpenStreetMap_and_Sentinel-2_CVPRW_2022_paper.pdf).

![this is an overview image](/img/dataset_teaser.png)

### Data Access

#### Azure Blob Storage (Free)

The dataset may be freely downloaded from Azure Blob Storage:

[Spatial Cell Metadata](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/spatial_cell_info.csv)

[OSM Label Categories](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/osm_categories.json)

[OSM Rasterized Label Images](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/osm_label_images.tgz)


[OSM Sentinel Imagery 2017](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/osm_sentinel_imagery_2017.tgz)

[OSM Sentinel Imagery 2018](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/osm_sentinel_imagery_2018.tgz)

[OSM Sentinel Imagery 2019](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/osm_sentinel_imagery_2019.tgz)

[OSM Sentinel Imagery 2020](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/osm_sentinel_imagery_2020.tgz)


[EuroSAT Sentinel L2A Resamples](https://vsipublic.blob.core.usgovcloudapi.net/vsi-open-sentinel-map/EuroSAT_sentinel2.tar.gz)


#### AWS (Paid)

As a backup option, or for faster download speeds, the dataset is also available on Amazon S3. You can use the following command to download it, but beware that Amazon will charge your AWS profile about $40 in data transfer fees (about 9 cents a GB, and 445 GB in total). NOTE: This option will be deprecated soon in favor of Azure Blob Storage.

```
aws s3 cp s3://vsi-open-sentinel-map/ ./open-sentinel-map --recursive --request-payer
```

### Data Format

#### Imagery

Image data is separated by year from 2017 to 2020. Each year's worth of Sentinel imagery is compressed into a osm_sentinel_imagery_{YEAR}.tgz file. These files can be untarred using the following command.
```
tar -xvzf osm_sentinel_imagery_{YEAR}.tgz
```
The untarred folders of sentinel imagery will have the format
```
MGRS_TILE/
    SPATIAL_CELL/
        {ID}_{YEAR}.npz
```
where each .npz file is a compressed numpy file containing the 32-bit float Bottom-of-Atmosphere imagery data. This file can be loaded from python using the numpy.load function, and the bands accessed via their keys. The bands are grouped by spatial resolution, and accessible using the key "gsd_{RESOLUTION}" (i.e. "gsd_10", "gsd_20", "gsd_60").

Note: The original Sentinel-2 data is stored as unsigned 16-bit integers. Our dataset converts to 32-bit floats and applies the Sentinel-2 scaling factor (divison by 10,000) to retrieve surface reflectance values. Although this should result in values from 0 to 1, some values will exceed 1 due to small errors in the data. We decided to keep these values greater than 1 for training robustness.

The "gsd_10" array bands have the order blue, green, red, and then NIR. The "gsd_20" bands have 4 vegetation red edge bands, followed by two SWIR bands. The "gsd_60" array consists of the coastal aerosol and water vapour bands. The exact corresponding bands from the Sentinel-2 platform are listed in the below table. Find more information about these spectral bands [here](https://gisgeography.com/sentinel-2-bands-combinations/).

| Data Key | Sentinel-2 Bands |
| -------- | ---------------- |
| gsd_10   | B02, B03, B04, B08 |
| gsd_20   | B05, B06, B07, B8A, B11, B12 |
| gsd_60   | B01, B09 |

The image files also contain an "scl" band and a "bad_percent" value. The "scl" band contains the Scene Classification Layer values, which inform the quality of each pixel at 20 m. resolution. These values are described in Figure 3 [here](https://sentinels.copernicus.eu/web/sentinel/technical-guides/sentinel-2-msi/level-2a/algorithm).

The "bad_percent" value is a float value between 0 and 1 which describes the percentage of pixels within the "scl" band which we've determined to be bad data. Currently we include images with up to 25% bad data. You can use this key to filter the dataset using a lower threshold.

#### Annotations

The label images can be untarred using the command
```
tar -xvzf osm_label_images.tgz
```

These images are in PNG format, with label values as described in the osm_categories.json file.

#### Auxiliary Data

The spatial_cell_info CSV file contains metadata for each spatial cell: the lon/lat bounds, the MGRS tile it is within, and the training split it belongs to. Note that the current data split was performed at the MGRS tile level to prevent data leakage. Use caution if performing your own train/test split.

The osm_categories JSON file details the exact mapping from OpenStreetMap tags to OpenSentinelMap labels.

### Licenses

This dataset is made available under the MIT license, freely available for both academic and commercial use.

Access to Sentinel data is free, full and open for the broad Regional, National, European and International user community. View [Terms and Conditions](https://scihub.copernicus.eu/twiki/do/view/SciHubWebPortal/TermsConditions).

OpenStreetMap® is _open data_, licensed under the [Open Data Commons Open Database License](https://opendatacommons.org/licenses/odbl/) (ODbL) by the [OpenStreetMap Foundation](https://wiki.osmfoundation.org/wiki/Main_Page) (OSMF).

### Contact

[email](mailto:dan@visionsystemsinc.com)

### How to Cite

bibtex:
```
@InProceedings{Johnson_2022_CVPR,
    author    = {Johnson, Noah and Treible, Wayne and Crispell, Daniel},
    title     = {OpenSentinelMap: A Large-Scale Land Use Dataset Using OpenStreetMap and Sentinel-2 Imagery},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) Workshops},
    month     = {June},
    year      = {2022},
    pages     = {1333-1341}
}
```

### Acknowledgements

This research is based upon work supported in part by the Office of the Director of National Intelligence (ODNI), Intelligence Advanced  Research Projects Activity (IARPA), via 2021-2011000004. The views and conclusions contained herein are those of the authors and should not be interpreted as necessarily representing the official policies, either expressed or implied, of ODNI, IARPA, or the U.S. Government. The U.S. Government is authorized to reproduce and distribute reprints for governmental purposes notwithstanding any copyright annotation therein.
