## OpenSentinelMap

The OpenSentinelMap dataset contains Sentinel-2 imagery and per-pixel OpenStreetMap label masks. It is described in [this paper](https://lmgtfy.app/?q=todo).

![this is an overview image](/img/dataset_teaser.png)

### Quick Start

[SharePoint link](https://vsi.sharepoint.us/:f:/r/sites/PublicShare/Shared%20Documents/vsi-open-sentinel-map?csf=1&web=1&e=od0eHA)

Or use the following command to download from AWS S3.
```
aws s3 cp s3://vsi-open-sentinel-map/ ./open-sentinel-map --recursive --request-payer
```

The dataset is about 445 GB in total. Amazon will charge your AWS profile about $40 (at 9 cents a GB) in data transfer fees.

### Data Format

The data is broken up into years from 2017 to 2020. Each year's worth of Sentinel imagery is compressed into a osm_sentinel_imagery_{YEAR}.tgz file. These files can be untarred using the following command.
```
tar -xvzf osm_sentinel_imagery_{YEAR}.tgz
```
The untarred folders of sentinel imagery will have the format
```
MGRS_TILE/
    SPATIAL_CELL/
        {ID}_{YEAR}.npz
```
where each .npz file is a compressed numpy file containing the 32-bit float Bottom-of-Atmosphere imagery data. The bands are grouped by resolution, and accessible using the key gsd_{RESOLUTION} (i.e. "gsd_10", "gsd_20", "gsd_60").

The "gsd_10" array bands have the order blue, green, red, and then NIR. The "gsd_20" bands have 4 vegetation red edge bands, followed by two SWIR bands. The "gsd_60" array consists of the coastal aerosol and water vapour bands. The exact corresponding bands from the Sentinel-2 platform are listed in the below table. Find more information about these spectral bands [here](https://gisgeography.com/sentinel-2-bands-combinations/).

| Data Key | Sentinel-2 Bands |
| -------- | ---------------- |
| gsd_10   | B02, B03, B04, B08 |
| gsd_20   | B05, B06, B07, B8A, B11, B12 |
| gsd_60   | B01, B09 |

### Download Instructions

Make sure you have the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) installed.

Configure your default AWS profile. If you wish to use the non-default, add the "--profile YOUR_PROFILE" option to all of the following aws commands.

View all files within the S3 bucket containing the dataset using the following command:
```
aws s3 ls s3://vsi-open-sentinel-map/ --request-payer
```

aws s3 cp s3://vsi-open-sentinel-map/osm_categories_v10.json . --request-payer

TODO: Download from [this SharePoint folder.](https://vsi.sharepoint.us/:f:/s/PublicShare/EnWfIp2gvi1As4tTZPzg1RcBYjtczFII9oWkU3MlbMDF9A?e=SYrIga)

### Licenses

Data is licensed under XXX.

Data is made available as-is, etc.

Access to Sentinel data is free, full and open for the broad Regional, National, European and International user community. View [Terms and Conditions](https://scihub.copernicus.eu/twiki/do/view/SciHubWebPortal/TermsConditions).

OpenStreetMap® is _open data_, licensed under the [Open Data Commons Open Database License](https://opendatacommons.org/licenses/odbl/) (ODbL) by the [OpenStreetMap Foundation](https://wiki.osmfoundation.org/wiki/Main_Page) (OSMF).

TODO: Do we also have to use the ODbL? Is this work a "derivative database"? Unclear.

### Contact

[email](mailto:noah.rego.johnson@gmail.com)

### How to Cite

bibtex:
```
@InProceedings{Johnson_2022_CVPR,
    author    = {Johnson, Noah and Treible, Wayne and Crispell, Daniel},
    title     = {OpenSentinelMap: A Large-Scale Land Use Dataset using OpenStreetMap and Sentinel-2 Imagery},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) Workshops},
    month     = {June},
    year      = {2022},
    pages     = {XXX-XXX}
}
```

### Acknowledgements

This research is based upon work supported in part by the Office of the Director of National Intelligence (ODNI), Intelligence Advanced  Research Projects Activity (IARPA), via 2021-2011000004. The views and conclusions contained herein are those of the authors and should not be interpreted as necessarily representing the official policies, either expressed or implied, of ODNI, IARPA, or the U.S. Government. The U.S. Government is authorized to reproduce and distribute reprints for governmental purposes notwithstanding any copyright annotation therein.
