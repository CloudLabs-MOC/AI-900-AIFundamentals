# Lab 02: Detect objects with Azure AI Custom Vision

## Estimated Duration: 60 Minutes

## Overview

Object detection is a form of computer vision in which a machine learning model is trained to classify individual instances of objects in an image, and indicate a *bounding box* that marks its location. You can think of this as a progression from *image classification* (in which the model answers the question "What is this an image of?") to building solutions where we can ask the model, "What objects are in this image, and where are they?"

For example, a road safety initiative might identify pedestrians and cyclists as being the most vulnerable road users at traffic intersections. By using cameras to monitor intersections, images of road users could be analyzed to detect pedestrians and cyclists in order to monitor their numbers or even change the behavior of traffic signals.

The **Custom Vision** Azure AI service in Microsoft Azure provides a cloud-based solution for creating and publishing custom object detection models. In Azure, you can use the Custom Vision service to train an object detection model based on existing images. There are two elements to creating an object detection solution. First, you must train a model to detect the location and class of objects using labelled images. Then, when the model is trained, you must publish it as a service that can be consumed by applications.

To test the capabilities of the Custom Vision service to detect objects in images, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, such as websites or mobile apps.

## Lab Objectives

You will be able to complete the following tasks:

  - Task 1: Create a Custom Vision project
  - Task 2: Add and tag images
  - Task 3: Train and test a model
  - Task 4: Publish the object detection model
  - Task 5: Prepare a client application
  - Task 6: Test the client application

## Task 1: Create a Custom Vision project

To train an object detection model, you need to create a Custom Vision project based on your training resource. To do this, you'll use the Custom Vision portal.

1. In the Azure Portal, using the search bar, search for **`Custom vision` (1)**, select **Custom vision (2)** from the result.

    ![](../media/createcv1.png)

1. On **AI Foundry | Custom vision** blade, click on **+ Create**.

    ![](../media/createcv2.png)

1. Provision the resource using the following settings and then click on **Review + create**:

    - Create options: **Both (1)**
    - Subscription: **Choose Default Subscription (2)**
    - Resource group: **AI-102-RG27 (3)**
    - Region: **<inject key="Region" enableCopy="false" /> (4)**
    - Name: **customvision<inject key="DeploymentID" enableCopy="false"/> (5)**
    - Training pricing tier: **F0 (6)**
    - Prediction pricing tier: **F0 (7)**

    ![](../media/createcv1.png)

1. On the **Review + create** tab, click **Create** to provision the resource.

    ![](../media/createcv2.png)

    ![](../media/createcv3.png)

1. Create the resource and wait for deployment to complete, and then view the deployment details. Note that two Custom Vision resources are provisioned; one for training, and another for prediction.

    > **Note**: Each resource has its own *endpoint* and *keys*, which are used to manage access from your code. To train an image classification model, your code must use the *training* resource (with its endpoint and key); and to use the trained model to predict image classes, your code must use the *prediction* resource (with its endpoint and key).

1. When the resources have been deployed, click on **Go to resource group** to view them.

    ![](../media/gotprg.png)

1. You should see two custom vision resources, one with the **-Prediction**.

    ![](../media/cust2.png)

1. In a new browser tab, open the Custom Vision portal at [https://customvision.ai](https://customvision.ai?azure-portal=true)

1. Click on **Sign in.**

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/gt17.png)

1. If prompted, sign in using the Microsoft account associated with your Azure subscription.

1. In the **Terms of Service** box, check the box **(1)** and click on **I agree (2)**.

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/2-7-25-m2-1.png)

1. On the **Custom Vision** page, click **NEW PROJECT**

   ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/2-7-25-m2-2.png)

1. Create a new project with the following settings:

    - Name: **Traffic Safety** **(1)**
    - Description: **Object detection for road safety** **(2)**
    - Resource: **aiservice-<inject key="DeploymentID" enableCopy="false"/> [SO]** **(3)**

        >**Note**: Under the **Resource** dropdown, if you don't find the resource that you created previously in the Azure portal, kindly refresh the page and re-perform the task.    

    - Project Types: **Object Detection** **(4)**
    - Domains: **General \[A1]** **(5)**
    - Click on **Create Project (6)**

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/createproj.png)
    
1. Wait for the project to be created and opened in the browser.

## Task 2: Add and tag images

To train an object detection model, you need to upload images that contain the classes you want the model to identify, and tag them to indicate bounding boxes for each object instance.

1. Copy the link and open it to download the zip file of images [https://aka.ms/traffic-images](https://aka.ms/traffic-images).

1. Click on the **Download** icon **&#11015; (1)** at the top of your browser. Then, click the **Folder** icon **&#128193;  (2)** to open the folder containing the downloaded **road-safety.zip** file.

    ![](../media/gt21.png)

1. Right click on the **road-safty zip file** **(1)** and select **Extract all (2)** to extract the training images. 

    ![](../media/gt22.png)

1. In the **Extract Compressed (Zipped) Folders** window, click **Extract (3)** to unzip the contents to the specified folder. 

   ![](../media/2-7-25-m2-4.png)

1. In the Custom Vision portal, in your **Traffic Safety** object detection project, select **Add images** and upload all of the images in the extracted folder.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg7.png)

1. Navigate to `C:\Users\demouser\Downloads\road-safety` **(1)**, select all the images (you can use **Ctrl+A** to select all the images) **(2)** and select **Open (3)**.

    ![](../media/2-7-25-m2-5.png)

1. Select **Upload 33 files** to begin uploading the selected images.

    ![](../media/gt24.png)

1. Select **Done**.

    ![](../media/gt25.png)

1. After the images have been uploaded, select the first one to open it.

    ![](../media/gt26.png)

1. Hold the mouse over any object (cyclist or pedestrian) in the image until an automatically detected region is displayed **(1)**. Then select the object, and if necessary, resize the region to surround it. Alternatively, you can simply drag around the object to create a region.

   When the object is tightly selected within the rectangular region, enter the appropriate tag for the object (**Cyclist** or **Pedestrian**) **(2)** and use the **Tag region (+) (3)** button to add the tag to the project.

     ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](../media/gt30.png)

1. Use the **Next** **(>)** button on the right to go to the next image, and tag its objects. Then just keep working through the entire image collection, tagging each cyclist and pedestrian.

    ![](../media/gt29.png)

    As you tag the images, note the following:

    - Some images contain multiple objects, potentially of different types. Tag each one, even if they overlap.
    - After a tag has been entered once, you can select it from the list when tagging new objects.
    - You can go back and forward through the images to adjust tags.

        ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](../media/multiple-objects-3b.png)

1. When you have finished tagging the last image, close the **Image Detail** editor, and on the **Training Images** page, under **Tags**, select **Tagged (1)** to view all your tagged images, such as **Cyclist** and **Pedestrian** **(2)**.

      ![Picture1](../media/gt31.png)

      >**Note:** Make sure that each tag has at least **15** entries **(2)**. If this requirement is not met, you will not be able to train the model.

## Task 3: Train and test a model

Now that you've tagged the images in your project, you're ready to train a model.

1. In the Custom Vision project, click the **&#9881; Train** button to begin training your object detection model using the tagged images.
   
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg8.png)
   
1. Select the **Quick Training** option **(1)** to train your model using the default training settings. Then, click **Train** **(2)** to begin the training process.
 
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/2-7-25-m2-6.png)
  
    > **Tip**: Training may take around **10 mins**. While you're waiting, check out [Video analytics for smart cities](https://www.microsoft.com/research/video/video-analytics-for-smart-cities/), which describes a real project to use computer vision in a road safety improvement initiative.

1. When training is complete, review the **Precision**, **Recall**, and **mAP** performance metrics - these measure the prediction goodness of the object detection model, and should all be reasonably high.

    ![](../media/gt33.png)

1. Adjust the **Probability Threshold** on the left, increasing it from `50% to 90%` **(1)**, and observe the effect on the performance metrics. This setting determines the probability value that each tag evaluation must meet or exceed to be counted as a prediction **(2)**.

      ![Screenshot of performance metrics for a trained model.](../media/gt34.png)

1. At the top right of the page, click **&#128504; Quick Test**.

      ![Screenshot of performance metrics for a trained model.](../media/2-7-25-m2-7.png)

1. Then in the **Image URL (1)** box, enter `https://aka.ms/pedestrian-cyclist`, click on the send icon **&#10145; (2)** and view the results.      

    ![](../media/2-7-25-m2-8.png)
      
    In the pane on the right, under **Predictions**, each detected object is listed with its tag and probability. Select each object to see it highlighted in the image.

    The predicted objects may not all be correct - after all, cyclists and pedestrians share many common features. The predictions that the model is most confident about have the highest probability values. Use the **Threshold Value** slider to eliminate objects with a low probability. You should be able to find a point at which only correct predictions are included (probably at around 85-90%).

      ![Screenshot of performance metrics for a trained model.](../media/test-detection-3b.png)

6. Then close the **Quick Test** window.

## Task 4: Publish the object detection model

Now you're ready to publish your trained model and use it from a client application.

1. In the **top-left corner** of the **Performance** tab, click the **&#128504; Publish** button to make the trained iteration available for predictions.


   ![Photograph of a group of pedestrians.](../media/2-7-25-m2-9.png)
 
1. Publish the trained model with the following settings:
    
    - Model name: **traffic-safety** **(1)**
    - Prediction resource: Select **aiservice-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Click **Publish (3)**

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/2-7-25-m2-10.png)

1. After publishing, click the **Prediction URL** (&#127760;) icon to see the information required to use the published model.

   ![](../media/gt37.png)
       
1. Later, you will need the **appropriate URL** and **Prediction-Key** values to get a prediction from an Image URL, so keep this dialog box open and carry on to the next task.

   ![](../media/gt38.png)

## Task 5: Prepare a client application

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the cloud shell on Azure.

1. Switch back to the browser tab containing the Azure portal, where the **Cloud shell** (**[>_]**) is already opened.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/powershell-portal-guide-1.png)

1. If you see the Cloud Shell timed out window, select **Reconnect**; otherwise, proceed with the next Task.   

   ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab4-2.png)

1. If the Code editor is closed, then only run the below command. The files are downloaded to a folder named **ai-search**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:

    ```PowerShell
    code .
    ```
    Notice how this opens up an editor like the one in the image below:

    ![The code editor.](../media/analyze-images-computer-vision-service/powershell-portal-guide-4(2).png)

    > **Tip**: You can use the separator bar between the cloud shell command line and the code editor to resize the panes.

    >**Tip**: If the code editor is still not opened, please run `code .` command again.

1. In the **Files** pane on the left, expand **ai-search (1)** and select **detect-objects.ps1 (2)**. 

   ![](../media/2-7-25-m2-11.png)

1. Don't worry too much about the details of the code. The important thing is that it starts with some code to specify the prediction URL and key for your Custom Vision model. You'll need to update these so that the rest of the code uses your model.

    Click on the **Prediction URL** to get the **Prediction URL (1)** and **Prediction key (2)** from the dialog box you left open in the browser tab for your Custom Vision project. You need the versions to be used *if you have an image URL*.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/2-7-25-m2-12.png)

    Use these values to replace the **YOUR_PREDICTION_URL** and **YOUR_PREDICTION_KEY** placeholders in the code file.
    After pasting the Prediction URL and Prediction Key values, the first two lines of code should look similar to this:

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/2-7-25-m2-13.png)    

1. After making the changes to the variables in the code, press **CTRL+S** to save the file. 

## Task 6: Test the client application

Now you can use the sample client application to detect cyclists and pedestrians in images.

1. Make sure you are in the **ai-search** folder. If not, run the below command to move into the folder.

    ```PowerShell
    cd ai-search
    ```

1. In the PowerShell pane, enter the following command to run the code:

    ```PowerShell
    ./detect-objects.ps1 1
    ```

    This code uses your model to detect objects in the following image:

     ![Photograph of a pedestrian and a cyclist.](../media/create-object-detection-solution/road-safety-1.jpg)

1. Review the prediction, which lists any objects detected with a probability of 90% or more, along with the coordinates of a bounding box around their location.

    ![Photograph of a group of pedestrians.](../media/gt42.png)

1. Now let's try another image. Run this command:

    ```PowerShell
    ./detect-objects.ps1 2
    ```

    This time, the following image is analyzed:

    ![Photograph of a group of pedestrians.](../media/create-object-detection-solution/road-safety-2.jpg)

1. Review the prediction, which lists any objects detected with a probability of 90% or more, along with the coordinates of a bounding box around their location.

    ![Photograph of a group of pedestrians.](../media/gt43.png)    

Hopefully, your object detection model did a good job of detecting pedestrians and cyclists in the test images.
   
   >**Note**: If you are not able to see the Result in PowerShell, then navigate back to the custom vision portal, go to the **predictions** tab, and you will see the details of the images as shown below:

   ![Photograph of a group of pedestrians.](../media/ai900mod3bimg12.png)

<validation step="3ee17fce-96a5-444e-8e5d-5e28cd8024a7" />

## Summary

In this lab, you have covered the following:
  
  - Created a Custom Vision project
  - Added and tagged images
  - Train and test a model
  - Published the object detection model
  - Prepared a client application
  - Tested the client application

## Learn more

This exercise shows only some of the capabilities of the Custom Vision service. To learn more about what you can do with this service, see the [Custom Vision page](https://learn.microsoft.com/en-us/azure/ai-services/custom-vision-service/).

### You have successfully completed the lab. Click on Next from the bottom right corner.

![Create storage by clicking confirm.](../media/gg3.png)
