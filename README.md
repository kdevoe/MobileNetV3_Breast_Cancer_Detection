# Tumor Classification Using MobileNetV3 with Transfer Learning

## Overview

The use of transfer learning to classify breast tumors was investigated using Google's [MobileNetV3](https://blog.research.google/2019/11/introducing-next-generation-on-device.html?m=1) pre-trained model together with transfer learning using images the Breast Cancer Histopathological Database [BreakHis](https://web.inf.ufpr.br/vri/databases/breast-cancer-histopathological-database-breakhis/).

<img width="440" alt="Screenshot 2023-10-01 at 10 49 46 AM" src="https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/fedfeb43-443b-435e-9116-39516eff6941">

(Image from [BreakHis](https://web.inf.ufpr.br/vri/databases/breast-cancer-histopathological-database-breakhis/) dataset site)



## Recommended installation

Run all commands from root directory of the project in a terminal:

1. Create a virtual environment

virtualenv env
source env/bin/activate

2. Install the requirements.txt file

env/bin/pip install -r requirements.txt

3. Run the mobilenet_v3 file in the notebook directly

