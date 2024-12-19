# Lab 02: Detect objects with Azure AI Custom Vision

## Estimated Duration: 60 minutes

## Overview

*Object detection* is a form of computer vision in which a machine learning model is trained to classify individual instances of objects in an image, and indicate a *bounding box* that marks its location. You can think of this as a progression from *image classification* (in which the model answers the question "What is this an image of?") to building solutions where we can ask the model "What objects are in this image, and where are they?"

For example, a road safety initiative might identify pedestrians and cyclists as being the most vulnerable road users at traffic intersections. By using cameras to monitor intersections, images of road users could be analyzed to detect pedestrians and cyclists in order to monitor their numbers or even change the behavior of traffic signals.

The **Custom Vision** Azure AI service in Microsoft Azure provides a cloud-based solution for creating and publishing custom object detection models. In Azure, you can use the Custom Vision service to train an object detection model based on existing images. There are two elements to creating an object detection solution. First, you must train a model to detect the location and class of objects using labelled images. Then, when the model is trained you must publish it as a service that can be consumed by applications.

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

1. In a new browser tab, open the Custom Vision portal at [https://customvision.ai](https://customvision.ai?azure-portal=true), and sign in using the Microsoft account associated with your Azure subscription.
1. In the **Terms of Service** box, check the box and click on **I agree**.

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg4.png)

1. Click **New project**.

   ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg5.png)

1. Create a new project with the following settings:

    - **Name**: Traffic Safety **(1)**
    - **Description**: Object detection for road safety **(2)**
    - **Resource**: **aiservice-<inject key="DeploymentID" enableCopy="false"/> [SO]** (3)
    - **Project Types**: Object Detection **(4)**
    - **Domains**: General \[A1] **(5)**
        >**Note**: Under the **Resource** dropdown if you don't find the resource that you created previously in the Azure portal, kindly refresh the page and reperform the task.
    - Click on **Create Project (6)**

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab2-1.png)
    
3. Wait for the project to be created and opened in the browser.

## Task 2: Add and tag images

To train an object detection model, you need to upload images that contain the classes you want the model to identify, and tag them to indicate bounding boxes for each object instance.

1. Copy the link and open it to download the zip file of images [https://aka.ms/traffic-images](https://aka.ms/traffic-images). Go to the download folder and extract the training images. The extracted folder contains a collection of images of cyclists and pedestrians.

1. In the Custom Vision portal, in your **Traffic Safety** object detection project, select **Add images** and upload all of the images in the extracted folder.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg7.png)

1. After the images have been uploaded, select the first one to open it.

1. Hold the mouse over any object (cyclist or pedestrian) in the image until an automatically detected region is displayed. Then select the object, and if necessary resize the region to surround it. Alternatively, you can simply drag around the object to create a region.

    When the object is tightly selected within the rectangular region, enter the appropriate tag for the object (*Cyclist* or *Pedestrian*) and use the **Tag region** (**+**) button to add the tag to the project.

     ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](../media/tag-image-3b.png)

1. Use the **Next** **(>)** link on the right to go to the next image, and tag its objects. Then just keep working through the entire image collection, tagging each cyclist and pedestrian.

    As you tag the images, note the following:

    - Some images contain multiple objects, potentially of different types. Tag each one, even if they overlap.
    - After a tag has been entered once, you can select it from the list when tagging new objects.
    - You can go back and forward through the images to adjust tags.

        ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](../media/multiple-objects-3b.png)

1. When you have finished tagging the last image, close the **Image Detail** editor, and on the **Training Images** page, under **Tags**, select **Tagged** to see all of your tagged images:

      ![Picture1](../media/tagged-images-3b.png)

## Task 3: Train and test a model

Now that you've tagged the images in your project, you're ready to train a model.

1. In the Custom Vision project, click **Train** to train an object detection model using the tagged images. 

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg8.png)
   
1. Select the **Quick Training** option, Again click **Train**.
 
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg9.png)

  
    > **Tip**: Training may take several minutes. While you're waiting, check out [Video analytics for smart cities](https://www.microsoft.com/research/video/video-analytics-for-smart-cities/), which describes a real project to use computer vision in a road safety improvement initiative.

2. When training is complete, review the *Precision*, *Recall*, and *mAP* performance metrics - these measure the prediction goodness of the object detection model, and should all be reasonably high.

3. Adjust the **Probability Threshold** on the left, increasing it from 50% to 90%, and observe the effect on the performance metrics. This setting determines the probability value that each tag evaluation must meet or exceed to be counted as a prediction.

      ![Screenshot of performance metrics for a trained model.](../media/performance-metrics-3b.png)

4. At the top right of the page, click **Quick Test**, and then in the **Image URL** box, enter `https://aka.ms/pedestrian-cyclist` and view the results.

      ![Screenshot of performance metrics for a trained model.](../media/quicktest-3b.png)
      
    In the pane on the right, under **Pedictions**, each detected object is listed with its tag and probability. Select each object to see it highlighted in the image.

    The predicted objects may not all be correct - after all, cyclists and pedestrians share many common features. The predictions that the model is most confident about have the highest probability values. Use the **Threshold Value** slider to eliminate objects with a low probability. You should be able to find a point at which only correct predictions are included (probably at around 85-90%).

      ![Screenshot of performance metrics for a trained model.](../media/test-detection-3b.png)

5. Then close the **Quick Test** window.

## Task 4: Publish the object detection model

Now you're ready to publish your trained model and use it from a client application.

1. Click **&#128504; Publish**.

   ![Photograph of a group of pedestrians.](../media/ai900mod3bimg10.png)
 
1. To publish the trained model with the following settings:
    - **Model name**: traffic-safety **(1)**
    - **Prediction resource**: **aiservice-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Click **Publish (3)**

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab2-2.png)

1. After publishing, click the *Prediction URL* (&#127760;) icon to see the information required to use the published model.
       
Later, you will need the appropriate URL and Prediction-Key values to get a prediction from an Image URL, so keep this dialog box open and carry on to the next task.

## Task 5: Prepare a client application

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the cloud shell on Azure.

1. Switch back to the browser tab containing the Azure portal, where the **Cloud shell** (**[>_]**) is already opened.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/powershell-portal-guide-1.png)

1. The files are downloaded to a folder named **ai-search**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:

    ```PowerShell
    code .
    ```
    Notice how this opens up an editor like the one in the image below:

    ![The code editor.](../media/analyze-images-computer-vision-service/powershell-portal-guide-4(2).png)

    > **Tip**: You can use the separator bar between the cloud shell command line and the code editor to resize the panes.

1. In the **Files** pane on the left, expand **ai-search** and select **detect-objects.ps1**. 

1. Don't worry too much about the details of the code. The important thing is that it starts with some code to specify the prediction URL and key for your Custom Vision model. You'll need to update these so that the rest of the code uses your model.

    Click on the **Prediction URL** to get the **Prediction URL (1)** and **Prediction key (2)** from the dialog box you left open in the browser tab for your Custom Vision project. You need the versions to be used *if you have an image URL*.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab2-4.png)

    Use these values to replace the **YOUR_PREDICTION_URL** and **YOUR_PREDICTION_KEY** placeholders in the code file.
    After pasting the Prediction URL and Prediction Key values, the first two lines of code should look similar to this:

    
     > $predictionUrl="https..."   
     > $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."


6. After making the changes to the variables in the code, press **CTRL+S** to save the file. 

## Task 6: Test the client application

Now you can use the sample client application to detect cyclists and pedestrians in images.

1. In the PowerShell pane, enter the following command to run the code:

    ```PowerShell
    ./detect-objects.ps1 1
    ```

    >**Note**: Make sure your are in **ai-search** folder if not run **cd ai-search** command to move into the folder.

    This code uses your model to detect objects in the following image:

     ![Photograph of a pedestrian and a cyclist.](../media/create-object-detection-solution/road-safety-1.jpg)

1. Review the prediction, which lists any objects detected with a probability of 90% or more, along with the coordinates of a bounding box around their location.

1. Now let's try another image. Run this command:

    ```PowerShell
    ./detect-objects.ps1 2
    ```

    This time the following image is analyzed:

    ![Photograph of a group of pedestrians.](../media/create-object-detection-solution/road-safety-2.jpg)

 Hopefully, your object detection model did a good job of detecting pedestrians and cyclists in the test images.

   
   >**Note**: If you are not able to see the Result in Powershell, then navigate back to the custom vision portal, go to the **predictions** tab, you see the details of the images as showned below:

   ![Photograph of a group of pedestrians.](../media/ai900mod3bimg12.png)

<validation step="3ee17fce-96a5-444e-8e5d-5e28cd8024a7" />

## Summary

In this lab you have covered the following:
  
  - Created a Custom Vision project
  - Added and tag images
  - Trained and test a model
  - Published the object detection model
  - Prepared a client application
  - Tested the client application

## Learn more

This exercise shows only some of the capabilities of the Custom Vision service. To learn more about what you can do with this service, see the [Custom Vision page](https://learn.microsoft.com/en-us/azure/ai-services/custom-vision-service/).

### You have successfully completed the lab. Click on Next from the bottom right corner.