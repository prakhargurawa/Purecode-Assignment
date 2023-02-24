# Purecode-Assignment
Generate JSON information from image 


## Problem formation

To generate the desciption json for any new image. the json contains various information but first and more important process which needs highest accuracy is to classify the image into classes [Button, Card, Checkbox, Icon, Switch]

The problem will be sepreated into various classifcation and regression problem piece by piece one by one but first will be to classify an image into classes [Button, Card, Checkbox, Icon, Switch]. Once this is done each class will be handled differntly and their json structure are not same and need to be generated different feature as per their classes.

The problem will be mapped to a simple multi class classifcation problem.

## Machine learning classifier

Createing a machine learning classifier to classsify input image into classes [Button, Card, Checkbox, Icon, Switch] using transfer learning technique.

#### Limitation/Constraints

1. Latency can be improved using with some test with some other light weight models like Mobilenet or even not using transfer learning and simple deep learning models.
2. Need to experiment with hyperparametes such as regularizations, epochs.
3. Image preprocessing techniques can improve the efficeiency of model. Try testing CLAHE, greyscale etc.


Constraints : We are resizing image into 180*180 which can lead to loss to input data especially for Card etc.


### Inference script ( Scoring script in Azure ML)
The scoring script is a Python file (.py) that contains the logic about how to run the model and read the input data submitted by the batch deployment executor driver. Each model deployment has to provide a scoring script, however, an endpoint may host multiple deployments using different scoring script versions.
Reference : https://learn.microsoft.com/en-us/azure/machine-learning/how-to-batch-scoring-script

Before the deployment can take place, there are several things you need to do:

1. Register the model’s .pkl file to Azure Machine Learning
2. Prepare a scoring script
3. Create an inference config
4. Create a Docker image containing your machine learning App Service

Reference : https://joonasaijala.com/2021/05/31/how-to-easily-deploying-azure-machine-learning-models-to-azure-app-service/


## Predicting other parameters/information

Once we have predicted componentType now we will predict other individual aspects/parameter based on that. 

{
    "componentType": "Button",
    "componentProps": {
        "variant": "contained",
        "size": "medium",
        "sx": {
            "textTransform": "none",
            "height": "27.1875px",
            "bgcolor": "#F8F4EC",
            "color": "#F8171B",
            "fontSize": "18px",
            "fontWeight": 800,
            "width": "85.390625px"
        },
        "disableElevation": true
    }
}

#### Predicting variants

We can observe variants do not exists for switch, icon,checkbox so not bothered to predict those for these classes. For Card its either outlined or not existing in json, so we can create two classes (outlines, not exist) and make this is as binary classifcation problem and solve testing with starting with simple models like logistic regression, SVM and then neaural nets.
For botton there exist five possibilities of variant (contained, outlined, text, '', not exist) which can again be treated as a multi classification problem.

#### Predicting disableElevation
We can observe disableElevation only exist in for button and can be treated a binary classification problem with true and false as classes, for other we dont need to use these models. 

Similarly we can create classifcation model for other elements which are categorical in nature.

For example to detect font we can test with Adobe's DeepFont (Reference : https://www.researchgate.net/publication/279993747_DeepFont_Identify_Your_Font_from_An_Image) or else if we need something of own will need some research time.

####  For sx

"sx": {
            "textTransform": "none",
            "height": "27.1875px",
            "bgcolor": "#F8F4EC",
            "color": "#F8171B",
            "fontSize": "18px",
            "fontWeight": 800,
            "width": "85.390625px"
        }
        
It will be better not to treat this as traditional classification/regression problem but rather than treat this as computer vision image processing problem, where we woul extract information using image itselfs such as bgcolor,color, font, width etc.        


### Libraries used
Keras for deep learning, python for processing etc
