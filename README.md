# Breast Histopathology Image Classification Using MobileNetV3 with Transfer Learning

## Background / Summary

The use of transfer learning to classify breast tumor images was investigated using Google's [MobileNetV3](https://blog.research.google/2019/11/introducing-next-generation-on-device.html?m=1) pre-trained model together with transfer learning using the Breast Cancer Histopathological Database [BreakHis](https://web.inf.ufpr.br/vri/databases/breast-cancer-histopathological-database-breakhis/).

<img width="440" alt="Screenshot 2023-10-01 at 10 49 46 AM" src="https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/fedfeb43-443b-435e-9116-39516eff6941">

(Image from [BreakHis](https://web.inf.ufpr.br/vri/databases/breast-cancer-histopathological-database-breakhis/) dataset site)

Using transfer learning we were able to achieve an overall F1 score of 0.98 on the test set (inference) while keeping the model size small at ~3 million parameters. Malignant (cancerous) tumors were detected with 97% accuracy with a false-positive rate of 5%. Overall these results outperformed larger models which did not utilize transfer learning.

A brief summary follows here however please see our [full paper](https://github.com/kdevoe/aai501-su23-group-1/blob/4f5177c8cab4710d3f147d7ab51067fa02038e34/Classification%20of%20Breast%20Histopathology%20Images.pdf) or [code notebook](https://github.com/kdevoe/aai501-su23-group-1/blob/4f5177c8cab4710d3f147d7ab51067fa02038e34/mobilenet_v3_transfer_model.ipynb) for detailed results.

## Methods

### Model Architecture

MobileNetV3 large was utilized as the base architecture with custom layers added to generate a multiclass softmax output for 8 tumor types, 4 benign and 4 malignant. The total number of parameters for the model as just over 3 million.

<img width="490" alt="Screenshot 2023-10-01 at 2 55 19 PM" src="https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/bfa081ec-a046-4174-80f8-cb507dcf4929">

### Data

**Pre-training**: The MobileNetv3 models was pre-trained on the [ImageNet](https://www.image-net.org/index.php) dataset which consists of over 1 million images of 1000 different classes.
**Fine-tuning**: Our model was fine-tuned on a smaller dataset of 7909 images from the [BreakHis](https://web.inf.ufpr.br/vri/databases/breast-cancer-histopathological-database-breakhis/) dataset. 
We split the BreakHis images into 75% training, 15% validation, and 10% testing. Due to the small sample size data augmentation was implemented through horizonal flipping of images to reduce overfitting.

### Key Training Items

**Layer Freezing**: The pre-trained model was frozen up to a specified layer and all following layers were fine-tuned with the BreakHis dataset. Based on the results below we selected layer 150 of 268 as the optimal layer to start training at. This layer gave the optimal results in terms of preserving the pre-trained weights while allowing the model to learn the new dataset.

<img width="755" alt="Screenshot 2023-10-01 at 2 56 08 PM" src="https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/7002a28e-bc30-404a-b6c7-69e8ed3fb5c1">  

*Validation accuracy given the layer in which fine-tuning is initiated.* 

**Learning Rate Selection**: The optimal initial learning rate and decay rate were selected using a grid search technique. From the results below an initial learning rate of 0.001 and decay rate of 0.95 per epoch were selected based on the performance on the validation set.

![image](https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/f76a8672-41c0-462f-a5db-409a7a9589d3)  
*Validation accuracy after 13 epochs given an initial learning rate and decay rate.*

## Key Results

Overall simple classification of tumors as malignant (cancerous) or benign (non-cancerous) was achieved with an F1 score of 0.98, precison of 0.98 and recall of 0.97 . The model as able to correctly classify 97% of malignant tumors with a false-positive rate of 5% on benign tumors.

<img width="516" alt="Screenshot 2023-10-01 at 2 56 53 PM" src="https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/8a56d370-18e2-4fb9-9eb8-aea1f5a000bf">  

*Binary classification results of the model only looking at malignant vs benign.*

Looking more detailed into tumor sub-types the below confusion matrix shows the model is also generally capable of correctly classifying the 8 tumor sub-types (4 benign and 4 malignant).

![image](https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/6ea11a31-969d-4c41-9df3-3dea46d1c74d)  
*Multiclass classification results for the 8 tumor sub-types.*

![image](https://github.com/kdevoe/aai501-su23-group-1/assets/31428365/a60d383d-5231-42f1-ab96-0036ecf164f0)
*Receiver operating characteristic (ROC) scores for the 8 tumor sub-types. Area under the curve (AUC) values ranged from 0.97 to 0.99 .*

As a final note a manual review by the medical doctor and histopathologist on the team revealed that misclassified images were often due to either poor images or images of cellular material that is not useful for diagnosis. This shows that there is likely room for improvement in results given additional review of the data.

## Recommended installation

Run all commands from root directory of the project in a terminal:

1. Create a virtual environment

virtualenv env
source env/bin/activate

2. Install the requirements.txt file

env/bin/pip install -r requirements.txt

3. Download and install the [BreakHis dataset](https://web.inf.ufpr.br/vri/databases/breast-cancer-histopathological-database-breakhis/) in the root directory: 

3. Run the [mobilenet_v3](https://github.com/kdevoe/aai501-su23-group-1/blob/4f5177c8cab4710d3f147d7ab51067fa02038e34/mobilenet_v3_transfer_model.ipynb) notebook file to reproduce results.

