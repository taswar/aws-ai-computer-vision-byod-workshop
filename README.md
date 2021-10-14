# Amazon Rekognition Custom Labels - Turkish Flag Detection

> This workshop is an adaptation from the original Rekognition Workshop [Lab 2](https://rekognition-immersionday.workshop.aws/en/)

The focus will be adding image and video analysis to your applications using AI/ML (deep learning) technology that requires no machine learning expertise to use.

## Computer Vision at AWS
Computer vision allows machines to identify people, places, and things in images with accuracy at or above human levels with much greater speed and efficiency. Often built with deep learning models, it automates extraction, analysis, classification and understanding of useful information from a single image or a sequence of images. The image data can take many forms, such as single images, video sequences, views from multiple cameras, or three-dimensional data.

Applications are far-reaching, from identifying defects in high speed assembly lines - to autonomous robots - to the analysis of medical images - to identifying products and people in social media.

AWS offers the broadest and deepest set of [machine learning services](https://aws.amazon.com/machine-learning/#Explore_AWS_Machine_Learning_services) and supporting cloud [infrastructure](https://aws.amazon.com/machine-learning/infrastructure/?c=ml&sec=int), putting machine learning in the hands of every developer, data scientist and expert practitioner. AWS is helping more than 100.000 customers accelerate their machine learning journey.

With Amazon Rekognition Custom Labels, you can identify the objects and scenes in images that are specific to your business needs. For example, you can find your logo in social media posts, identify your products on store shelves, classify machine parts in an assembly line, distinguish healthy and infected plants, or detect animated characters in videos.

![Volley Ball Team](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/volleyball.png)


Rekognition Object Detection deals with finding objects within an image. To train your model, Amazon Rekognition Custom Labels require bounding boxes to be drawn around objects and the objects should be labeled in your images.

In this lab, we will detect Turkish flag on the Turkish Women Volley Ball team within images using Amazon Rekognition Custom Labels.

# Target Audience
Software developers, solutions architects, product managers with no experience in Data Science and Machine Learning. A basic.
A basic knowledge of Python and Jupyter Notebooks, and basic AWS knowledge is recommended but not required.

## Prerequisites
AWS Account
You will be performing the real-life scenarios and activities in  AWS account. Knowledge about AWS basics (AWS console, S3) are required.
1. Bring your own Account.
2. AWS teams delivering the workshop will provide you with temporary accounts.

Bring Your Own Data
You could use 10-50 representative images for your use case. Supported image formats: JPG, PNG. Maximum image size: 15 MB, Minimum size (px): 64 x 64. Maximum size (px): 4096 x 4096. If you don’t have any data, you could use the sample dataset provided in this workshop.


# Preprocessing
If your image has an object, such as a machine part or an animated character, the image needs a bounding box around an object and an object-identifying label. You can have multiple objects within an image. In this step, you add object-level labels and bounding boxes to an image.

Before you start with the lab, download the images from the below compressed file:

Sample dataset  
------------  
> [filenin-sultanlari.zip](https://github.com/taswar/aws-ai-computer-vision-byod-workshop/blob/main/filenin-sultanlari.zip?raw=true) (4.24 M) 


## Upload data to S3

To make the labeling process easy, upload and organize the images in a single folder (such as **tr-flag**) in S3. The path for the images would be similar to **rekognitioncustomlabels/tr-flag/**.

Supported file formats are PNG and JPEG. The maximum number of images per dataset is 250,000. Make sure that the minimum image dimension of each image file 64 pixels x 64 pixels, and the maximum is 4096 pixels x 4096 pixels.

Other limits are specified [here](https://docs.aws.amazon.com/rekognition/latest/customlabels-dg/limits.html).

**Step 1: Go to  S3**
- Search for S3 and open it in a new browser tab
- Create a bucket e.g volleyball-username-dd-mm-yyyy (ie. *volleyball-taswar-01-01-2021*) Host it in the N.Virginia us-east-1 region
![s3](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/creates3bucket.png)
- Leave the rest as default (you do not need public access or encyrption or tags)

**Step 2: Create folder**
- Once the bucket is created then create a folder in the bucket (tr-flag)
![23 folder](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/creates3folder.png)

**Step 3: Upload images**
- Upload all the images from the zip file into the folder (Drag and drop all the images in the upload section)
![Uploadedfiles](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/s3uploadedfiles.png)

**Step 4: Leave S3 open**
- Leave the S3 console tab open and jump back to the other console tab of AWS for next part, we will need the s3 section later for adding permission into this bucket.

---

## Create a dataset

**Step 1:** In your non S3 tab navigate to Rekognition on the console and click **“Amazon Rekognition”**:

![rekognition](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/navigatetorekognition.png)

**Step 2:** Click on Use **Custom Labels**.

![customlabels](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/clickcustomlabels.png)

**Step 3:** On the left sidebar / menu, click datasets. You may need to open the Hamburger menu.

![datasets](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/clickdatasetsmenu.png)

**Step 4:** You may see an image like below for first time setup.

![firsttimesetup](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/firsttimes3.png)

**Step 5:** Click on **Create Dataset**

![createdataset](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/createdataset.png)

**Step 6:** Provide a dataset name and choose **Import images from S3.**

![import s3](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/importimagesfroms3.png)

**Step 7:** Enter the S3 folder location: (e.g *s3://volleyball-taswar-01-01-2021/*) - Remember the "/" at the end of the location folder. Permission tab will auto be generated below the UI.

**Step 8:** **Switch to the S3 console**, copy and paste the bucket permissions into the bucket that contains your data: In the permission tab and click **Edit** to add the permission on the bucket. What this is doing is giving Rekcognition the permission to access the bucket.

![permission](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/pastebucketconfiguration.png)

**Step 9:** **Switch back to the Rekognition console**, the S3 path should already been filled, and check **Automatic labeling**, and click **Submit**.

**Step 10:** Now, as images are labeled automatically, you need to draw bounding boxes on the images. You will click on “Start Labeling” on the right top corner.

![Start labelling](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/startlabelling.png)

Once you’re in labeling mode, you will select the images and click on “Draw bounding box”. Select one page at a time, you can select all the images on page 1, once complete we will move to page 2 and do the exact same actions like below.

![Boundingboxes](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/boundingboximages.png)

You will be presented a preview of the image and labels on the right.

Label should be auto-selected (for example, **“tr-flag”**) and all you need to do is to draw bounding box around the object. As seen below I have draw around the flag and include **TUR** in the box.

![Drawingbox](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/drawingbox.png)
If you have multiple objects in an image, you will follow the same process of selecting the label and drawing bounding box around it.

Similarly, you will go through entire set of images page by page to draw bounding box around the objects. Once you’re done, you will click on **“Save changes”** on the right top corner to come out of labeling mode and saving the changes you made. Your dataset should show black boxes around the **tr-flag**.

---

# Training

## Note
> Make sure you have a usable dataset before you follow along here to train a custom model

Once you have a dataset, you can start by creating a project inside of Rekognition.

## Create a project

**Step 1:** On the left sidebar / menu, click Projects. You may need to open the Hamburger menu.

**Step 2:** On the projects page, click Create project.

**Step 3:** Provide a project name and click the Create project button.
![Create Project](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/createrekproject.png)

**Step 4:** Click **Train** new model.
![Train new model](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/trainnewmodel.png)

**Step 5:** Choose a training dataset in the dropdown, click Split training dataset, and click **Train**. We wnat 80% training 20% for testing
![Split dataset](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/splittrainingdatasets.png)

## Note Info
> The model training process will take **50-60 mins.** Continue to the next section.

---

# Inference

## Launching demo application 
As the training of the model from previous step continues, launch the Custom Labels demo application. We will use this web application to test the custom model.

You can use click on “Launch on AWS” button to launch the stack in your account. Make sure you use the same appropriate region as your model. In our case N.Virginia

[Amazon Rekognition Custom Labels Demo](https://github.com/aws-samples/amazon-rekognition-custom-labels-demo)

## Note
> Wait for the training to complete before you start with the following steps.

## Inference
Navigate to the project you trained your model under, wait for the **status** to show TRAINING_COMPLETE and then click the on the model **Name** of the trained model: 

Evaluate your model performance details. 

If you’re happy with the performance of your model, you make it available for use by starting it.
Inference using GUI-based application

In this approach, we will use the demo application we launched at the beginning of the section. Follow the steps below:

1. Open AWS console and switch to “us-east-1” region.
2. Under services search for “Amazon CloudFormation”.
3. Find the stack you launched for the demo application.
4. Go to the “Outputs” tab and copy the URL.
5. Paste the URL in a browser window.
6. You would have received the email with credential for the demo application. Use them to log in. You will be asked to change the password.

![Cloud formation output](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/Cloudformation-output-tab.png)

Once logged in, you will be presented with a page similar to below. 
![Custom UI](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/CustomLabelWebUI.png)

Click on **“Start the model”** for the model you just trained. Wait for the model to reach “RUNNING” state. 

Click on the model name. You will see a page similar to below. 
![Demo upload](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/demoapplicationtest.png)

Use the following images to test your model.
### Sample test dataset 
- [Test image 1](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/test-image1.jpg)
- [Test image 2](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/test-image2.jpg)
- [Test image 3](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/test-image3.jpg)
- [Test image 4](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/test-image4.jpg)

Upload the images one after the other to see if it can identify tr-flag logo.
![Test Result](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/result.png)

If the logo is detected, it will draw a bounding box around the tr-flag. It will also show the confidence score under **“Results”.** 
**Note** You may get a difference confidence score than the image below.
![Test Result Response](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/result-response.png)

Click on **“Projects”** to go back to main page. **“Stop the model”** once you’re done with testing.

# Disclaimer: 
Images, brands  used in this workshop are obtained from public domain photos and used solely for education purposes. AWS and the brands are not endorsed.
