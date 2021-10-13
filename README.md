# aws-ai-computer-vision-byod-workshop

# Amazon Rekognition Custom Labels - Turkish Flag Detection

With Amazon Rekognition Custom Labels, you can identify the objects and scenes in images that are specific to your business needs. For example, you can find your logo in social media posts, identify your products on store shelves, classify machine parts in an assembly line, distinguish healthy and infected plants, or detect animated characters in videos.

![Volley Ball Team](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/volleyball.png)


Rekognition Object Detection deals with finding objects within an image. To train your model, Amazon Rekognition Custom Labels require bounding boxes to be drawn around objects and the objects should be labeled in your images.

In this lab, we will detect Turkish flag on the Turkish Women Volley Ball team within images using Amazon Rekognition Custom Labels.

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

**Step 9:** **Switch back to the Rekognition console**, enter the S3 path, and check **Automatic labeling**, and click **Submit**.

**Step 10:** Now, as images are labeled automatically, you need to draw bounding boxes on the images. You will click on “Start Labeling” on the right top corner.

![Start labelling](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/startlabelling.png)

Once you’re in labeling mode, you will select the images and click on “Draw bounding box”.

![Boundingboxes](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/boundingboximages.png)

You will be presented a preview of the image and labels on the right.

Label should be auto-selected (for example, **“tr-flag”**) and all you need to do is to draw bounding box around the object.

![Drawingbox](https://raw.githubusercontent.com/taswar/aws-ai-computer-vision-byod-workshop/main/drawingbox.png)
If you have multiple objects in an image, you will follow the same process of selecting the label and drawing bounding box around it.

Similarly, you will go through entire set of images to draw bounding box around the objects. Once you’re done, you will click on “Save changes” on the right top corner to come out of labeling mode and saving the changes you made. Your dataset should look similar to below:

---

# Training
