# Agrolabs : Vision API 
> Prototype Vision API 

> Objective : To detect specific diseases in common crop plants and store the analysis for future solutions

> Team : Future AI Laboratory

> Deliverable : TF Lite model that can be embedded into a flutter app

> Folder structure for models
>   * models/potato/ @sayan
>   * models/citrus/ @sarthak


# **Potato Leaf Disease Classification Study and Deep Learning Pipeline Design for Computer Vision**

## **Introduction:**

In agricultural field, everything is natural from production to maintenance of crops, weather effects, etc. and lots of uncertainities involved, which require continious and efficient monitoring, and take quick action in situation basis, which is not feasible for individuals, thus technology introduces it's vision for those solutions. There maybe severl vision based applications, when it comes to agriculture. Our goal is to bring multiple vision base applications under the same hood, so that we are proposing to make a centralized cloud based application, which is as follow:

## **Objective** 

Our objective is to build a cloud platform, where multiple vision based cloud services will be run for classifying different plant leaf diseases. Here we are focusing on Potato Leaf Disease classification. 

## **Dataset**

* **Source:** [Mendley Plant Village Dataset](https://data.mendeley.com/datasets/tywbtsjrjv/1)

* **Description:** In this data-set, 39 different classes of plant leaf and background images are available. the Plant Village Dataset consists of two different sets of data 

  **1. Original Dataset** contains 54303 healthy and unhealthy leaf images. 
  
  **2. Augmented dataset** containing 61,486 images. Six different augmentation techniques were used for increasing the data-set size. The techniques are  >image flipping, Gamma correction, noise injection, PCA color augmentation, rotation, and Scaling.

* Original Paper: [An open access repository of images on plant health to enable the development of mobile disease diagnostics](https://arxiv.org/abs/1511.08060)

#### **Potato Dataset**

Among the 39 classes there are 3 following categories of Potato classes available in the dataset. 
 * **Potato Early Blight:** Early blight of potato is caused by the fungal pathogen Alternaria solani. The disease affects leaves, stems and tubers and can reduce yield, tuber size, storability of tubers, quality of fresh-market and processing tubers and marketability of the crop. More on this can be found [here](https://www.ag.ndsu.edu/publications/crops/early-blight-in-potato#:~:text=Early%20blight%20of%20potato%20is,and%20marketability%20of%20the%20crop.)
![EB](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/Potato_early_blight.png)
* **Potato Late Blight:** Late blight, also called potato blight, disease of potato plants that is caused by the water mold Phytophthora infestans. The disease occurs in humid regions with temperatures ranging between 4 and 29 °C (40 and 80 °F). Hot dry weather checks its spread. Potato or tomato plants that are infected may rot within two weeks. More on this can be found [here](https://cropwatch.unl.edu/potato/late_blights_description)
![Late blight](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/late_blight.png)
 * **Potato Healthy:** Potato Healthy Leaves are those, which have no diseases, which are fresh and healthy.
![Healthy](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/healthy_new.png) 

## **Class Distribution**

In original and augmented dataset contains 2152 and 3000 image samples respectively. The class and image shape distribution is as follows.

![Distribution](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/potato_distribution.PNG)

In original set Potato Healthy class contains 152 images, wheras the rest two classes contain 1000 images. The Augmented dataset contains 1000 classes each. For Original Dataset all the images are of standard resolution (256,256,3), whereas in Augmented dataset the 2152 images are of shape (256,256,3), and 848 images of class Potato_Healthy are of shape (204,204,3).  

## **Workflow**

We will build an entire pipeline which will help to fetch the data from the Plant Village Mendley site to Data Store, Data analysis, Data Preperation/Pre-processing, Train-Valid-Test Distribution, Model Building, Training, Tuning, Evaluation, Model Store, TFlite conversion. The workflow of the process is shown using a following UML diagram. 

![UML Diagram]()

## **Detailed Study**

We have tried to compare between the performance of the CNN models using Original and Augmented dataset. 

#### 1. Data Pre-processing 
The following data-preprocessing techniques are used-
   * Image_to_array: Converting image samples to numpy array
   * Interpolation: To interpolate all the images to standard shape (256,256,3) using "Bicubic", interpolation. 
   * Shuffle: Shuffle all the samples
   * Encoding: The labels are encoded with a class mode "sparse", to support the "sparse_categorical_cross-entropy"

#### 2. Train-Validation-Test split 
   * Ratio: 90% Train(with a Validation split = 0.1), 10% Test.
   * Split: TO overcome the class imabalance during train-test split, stratified train-test split is done using label. 

#### 3. CNN Model Building
Convolution Neural Network(CNN) model is used here for Leaf Disease Classification. We have started from the reference CNN model of the paper "[Comparative Assessment of Deep Learning to Detect the Leaf Diseases of Potato based on Data Augmentation](https://ieeexplore.ieee.org/abstract/document/9200015)". Then, we modified the CNN model and proposed a much more lighter and more consistent model. The comparison between refernce and propsed model is shown below.

![Model Comparison](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/Model_comparison.png)

From the model architecture it is clearly visible(Highlighted) that, the proposed model is much more lighter than the reference model. The model is diffivult to run in collab and log to wandb using wandb callback.

#### 4. Model Training Report

Model training report for original and augmented dataset

![Training report](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/Training%20report.png)

#### 5. Model Evaluation for Original and Augmented Dataset

The comparison of model performance is done for both original and augmenetd set to finalyze the dataset for training, and classification reportt is generated using test dataset. The model performance for the datasets are compared using Classification report, Confusion Matrix

* **Comparison Using Classification Report**

   ![Model result](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/classification_report.PNG)

#### 6. Weights and Biases Training Log
 
All the model trainings are logged using weights and biases callback to log model training metrices, system environment parameters, so that we can visualize and analyze that for later analysis, and wandb helps to identify the best epoch if the validation set is passed to wandbcllbacl, The demo image of the training logs logged in the wandb "Potato Leaf Disease Classifier" project is shown below-
   ![wandb_log](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/wandb%20log.PNG)
   
The detailed report for the Potato Leaf Disease Classifier can be found from the following link-
## [weights and biases reports](https://wandb.ai/sayan0506/Potato_disease_classifier)

* **Comparison using Confusion Matrix**  

   ![CM](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/confusion_matrix_updated.png)

#### Result Interepretation Using GradCAM(Gradient weighted Class Activation Map)
---
Class Activation Map analysis for Potato Healthy Image samples

![Potato_healthy CAM](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/potato_helthy_CAM.png)

Class Activation Map analysis for Potato Early Blight Image samples

![Potato early blight](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/early%20blight%20map.png)

Class Activation Map analysis for Potato Late Blight Image samples

![Potato Late Blight](https://github.com/Future-AI-Laboratory/vision-api/blob/review_sayan/images/Potato%20Late%20Blight%20Map.png)

**The detailed study of the Potato Leaf Disease classification can be found from the following colab notebook-**
# [potato_leaf_disease_clasifier_notebook.](https://colab.research.google.com/drive/1qudYwbuAXGypX9bQsUlrGlP59YTirY3B?usp=sharing)

## Deep Learning Pipeline Design

Detailed implementation and study regarding the Deep Learning Pipeline design from data-loader to tflite conversion can be found from the following link-

# **[Vision API Design](https://github.com/Future-AI-Laboratory/vision-api/blob/gh-pages/docs/index.md)**

# **Reference**

* [Comparative Assessment of Deep Learning to Detect the Leaf Diseases of Potato based on Data Augmentation](https://ieeexplore.ieee.org/abstract/document/9200015)
* [Weights and Biases](https://github.com/wandb/client)
