## OpenSentinelMap

The OpenSentinelMap dataset contains Sentinel-2 imagery and per-pixel semantic label masks derived from OpenStreetMap. It is described in [this paper](https://openaccess.thecvf.com/content/CVPR2022W/EarthVision/papers/Johnson_OpenSentinelMap_A_Large-Scale_Land_Use_Dataset_Using_OpenStreetMap_and_Sentinel-2_CVPRW_2022_paper.pdf).

![this is an overview image](/img/dataset_teaser.png)

### Data Access

The dataset may be freely downloaded from SharePoint [here.](https://vsi.sharepoint.us/:f:/s/PublicShare/EnWfIp2gvi1As4tTZPzg1RcBYjtczFII9oWkU3MlbMDF9A)

As a backup option, or for faster download speeds, the dataset is also available on Amazon S3. You can use the following command to download it, but beware that Amazon will charge your AWS profile about $40 in data transfer fees (about 9 cents a GB, and 445 GB in total).

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
where each .npz file is a compressed numpy file containing the 32-bit float Bottom-of-Atmosphere imagery data. The bands are grouped by spatial resolution, and accessible using the key "gsd_{RESOLUTION}" (i.e. "gsd_10", "gsd_20", "gsd_60").

The "gsd_10" array bands have the order blue, green, red, and then NIR. The "gsd_20" bands have 4 vegetation red edge bands, followed by two SWIR bands. The "gsd_60" array consists of the coastal aerosol and water vapour bands. The exact corresponding bands from the Sentinel-2 platform are listed in the below table. Find more information about these spectral bands [here](https://gisgeography.com/sentinel-2-bands-combinations/).

| Data Key | Sentinel-2 Bands |
| -------- | ---------------- |
| gsd_10   | B02, B03, B04, B08 |
| gsd_20   | B05, B06, B07, B8A, B11, B12 |
| gsd_60   | B01, B09 |

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

OpenStreetMap?? is _open data_, licensed under the [Open Data Commons Open Database License](https://opendatacommons.org/licenses/odbl/) (ODbL) by the [OpenStreetMap Foundation](https://wiki.osmfoundation.org/wiki/Main_Page) (OSMF).

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
