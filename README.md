
# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the  rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request,a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using  our  CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# SketchCode

![](https://img.shields.io/badge/python-3-brightgreen.svg) ![](https://img.shields.io/badge/tensorflow-1.1.0-orange.svg)

*Generating HTML Code from a hand-drawn wireframe*

![Preview](https://github.com/ashnkumar/sketch-code/blob/master/header_image.png)

SketchCode is a deep learning model that takes hand-drawn web mockups and converts them into working HTML code. It uses an [image captioning](https://towardsdatascience.com/image-captioning-in-deep-learning-9cd23fb4d8d2) architecture to generate its HTML markup from hand-drawn website wireframes.

For more information, check out this post: [Automating front-end development with deep learning](https://blog.insightdatascience.com/automated-front-end-development-using-deep-learning-3169dd086e82)

This project builds on the synthetically generated dataset and model architecture from [pix2code](https://github.com/tonybeltramelli/pix2code) by [Tony Beltramelli](https://github.com/tonybeltramelli) and the [Design Mockups](https://github.com/emilwallner/Screenshot-to-code-in-Keras) project from [Emil Wallner](https://github.com/emilwallner).

<b>Note:</b> This project is meant as a proof-of-concept; the model isn't (yet) built to generalize to the variability of sketches seen in actual wireframes, and thus its performance relies on wireframes resembling the core dataset.

## Setup
### Prerequisites

- Python 3 (not compatible with python 2)
- pip

### Install dependencies

```sh
pip install -r requirements.txt
```
## Example Usage

Download the data and pretrained weights:
```sh
# Getting the data, 1,700 images, 342mb
git clone https://github.com/ashnkumar/sketch-code.git
cd sketch-code
cd scripts

# Get the data and pretrained weights
sh get_data.sh
sh get_pretrained_model.sh
```

Converting an example drawn image into HTML code, using pretrained weights:
```sh
cd src

python convert_single_image.py --png_path ../examples/drawn_example1.png \
      --output_folder ./generated_html \
      --model_json_file ../bin/model_json.json \
      --model_weights_file ../bin/weights.h5
```
## General Usage

Converting a single image into HTML code, using weights:
```sh
cd src

python convert_single_image.py --png_path {path/to/img.png} \
      --output_folder {folder/to/output/html} \
      --model_json_file {path/to/model/json_file.json} \
      --model_weights_file {path/to/model/weights.h5}
```

Converting a batch of images in a folder to HTML:
```sh
cd src

python convert_batch_of_images.py --pngs_path {path/to/folder/with/pngs} \
      --output_folder {folder/to/output/html} \
      --model_json_file {path/to/model/json_file.json} \
      --model_weights_file {path/to/model/weights.h5}
```
