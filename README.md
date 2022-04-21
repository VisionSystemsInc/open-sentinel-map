## OpenSentinelMap

The OpenSentinelMap dataset contains Sentinel-2 imagery and per-pixel OpenStreetMap label masks. It is described in [this paper](https://lmgtfy.app/?q=todo).

![this is an overview image](/img/dataset_teaser.png)

### Quick Start

To download the entire dataset to your current directory via the command line, run the following:
```
aws s3 cp s3://vsi-open-sentinel-map/ . --recursive --request-payer
```

Your AWS profile will be charged around $10.

### Data Format

The data is broken up into years from 2017 to 2020. Each 

### Download Instructions

Make sure you have the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) installed.

Configure your default AWS profile. If you wish to use the non-default, add the "--profile YOUR_PROFILE" option to all of the following aws commands.

View all files within the S3 bucket containing the dataset using the following command:
```
aws s3 ls s3://vsi-open-sentinel-map/ --request-payer
```

aws s3 cp s3://vsi-open-sentinel-map/osm_categories_v10.json . --request-payer

TODO: Download using [this OneDrive link.](https://lmgtfy.app/?q=todo)

### Licenses

Data is licensed under XXX.

Data is made available as-is, etc.

Access to Sentinel data is free, full and open for the broad Regional, National, European and International user community. View [Terms and Conditions](https://scihub.copernicus.eu/twiki/do/view/SciHubWebPortal/TermsConditions).

OpenStreetMap® is _open data_, licensed under the [Open Data Commons Open Database License](https://opendatacommons.org/licenses/odbl/) (ODbL) by the [OpenStreetMap Foundation](https://wiki.osmfoundation.org/wiki/Main_Page) (OSMF).

TODO: Do we also have to use the ODbL? Is this work a "derivative database"? Unclear.

### Contact

[email](mailto:noah.rego.johnson@gmail.com)

### How to Cite

Latex:
```
@InProceedings{Nguyen_2021_CVPR,
    author    = {Nguyen, Ngoc Long and Anger, Jeremy and Davy, Axel and Arias, Pablo and Facciolo, Gabriele},
    title     = {Self-Supervised Multi-Image Super-Resolution for Push-Frame Satellite Images},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) Workshops},
    month     = {June},
    year      = {2021},
    pages     = {1121-1131}
}
```

### Acknowledgements

This research is based upon work supported in part by the Office of the Director of National Intelligence (ODNI), Intelligence Advanced  Research Projects Activity (IARPA), via 2021-2011000004. The views and conclusions contained herein are those of the authors and should not be interpreted as necessarily representing the official policies, either expressed or implied, of ODNI, IARPA, or the U.S. Government. The U.S. Government is authorized to reproduce and distribute reprints for governmental purposes notwithstanding any copyright annotation therein.
