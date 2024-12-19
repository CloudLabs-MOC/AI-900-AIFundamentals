# Lab 02: Detect Objects with Azure AI Custom Vision

## Estimated Duration: 60 Minutes

## Overview

*Object detection* is a form of computer vision in which a machine learning model is trained to classify individual instances of objects in an image and indicate a *bounding box* that marks its location. You can think of this as a progression from *image classification* (in which the model answers the question, "What is this an image of?") to building solutions where we can ask the model, "What objects are in this image, and where are they?"

For example, a road safety initiative might identify pedestrians and cyclists as the most vulnerable road users at traffic intersections. By using cameras to monitor intersections, images of road users could be analyzed to detect pedestrians and cyclists in order to monitor their numbers or even change the behavior of traffic signals.

The **Custom Vision** Azure AI service in Microsoft Azure provides a cloud-based solution for creating and publishing custom object detection models. In Azure, you can use the Custom Vision service to train an object detection model based on existing images. There are two elements to creating an object detection solution. First, you must train a model to detect the location and class of objects using labeled images. Then, when the model is trained, you must publish it as a service that can be consumed by applications.

To test the capabilities of the Custom Vision service to detect objects in images, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply to real-world solutions, like websites or mobile apps.

## Lab Objectives

You will be able to complete the following tasks:

  - **Task 1:** Create a Custom Vision Project
  - **Task 2:** Add and Tag Images
  - **Task 3:** Train and Test a Model
  - **Task 4:** Publish the Object Detection Model
  - **Task 5:** Prepare a Client Application
  - **Task 6:** Test the Client Application

### Task 1: Create a Custom Vision Project

To train an object detection model, you need to create a Custom Vision project based on your training resource. To do this, you will use the Custom Vision portal.

1. In a new browser tab, open the Custom Vision portal at [https://customvision.ai](https://customvision.ai?azure-portal=true) and sign in using the Microsoft account associated with your Azure subscription.
1. In the **Terms of Service** box, check the box and click on **I agree**.

     ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg4.png)

1. Click **NEW PROJECT**.

   ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg5.png)

1. Create a new project with the following settings:

    - **Name**: Traffic Safety **(1)**
    - **Description**: Object detection for road safety **(2)**
    - **Resource**: aiservice-<inject key="DeploymentID" enableCopy="false"/> [SO] **(3)**
    - **Project Types**: Object Detection **(4)**
    - **Domains**: General \[A1] **(5)**
        >**Note**: Under the **Resource** dropdown if you don't find the resource you created previously in the Azure portal, kindly refresh the page and perform the task again.
    - Click on **Create Project (6)**

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab2-1.png)
    
3. Wait for the project to be created and opened in the browser.

### Task 2: Add and Tag Images

To train an object detection model, you need to upload images that contain the classes you want the model to identify and tag them to indicate bounding boxes for each object instance.

1. Copy the link and open it to download the zip file of images: [https://aka.ms/traffic-images](https://aka.ms/traffic-images). Go to the download folder and extract the training images. The extracted folder contains a collection of images of cyclists and pedestrians.

1. In the Custom Vision portal, within your **Traffic Safety** object detection project, select **Add images** and upload all of the images to the extracted folder.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg7.png)

1. After the images have been uploaded, select the first one to open it.

1. Hold the mouse over any object (cyclist or pedestrian) in the image until an automatically detected region is displayed. Then, select the object, and if necessary, resize the region to surround it. Alternatively, you can simply drag around the object to create a region.

    When the object is tightly selected within the rectangular region, enter the appropriate tag for the object (*Cyclist* or *Pedestrian*) and use the **Tag region** (**+**) button to add the tag to the project.

     ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](../media/tag-image-3b.png)

1. Use the **Next** **(>)** link on the right to go to the next image and tag its objects. Then just keep working through the entire image collection, tagging each cyclist and pedestrian.

    As you tag the images, note the following:

    - Some images contain multiple objects, potentially of different types. Tag each one, even if they overlap.
    - After a tag has been entered once, you can select it from the list when tagging new objects.
    - You can go back and forth through the images to adjust tags.

        ![Screenshot of an image with a tagged region in the Image Detaol dialog box.](../media/multiple-objects-3b.png)

1. When you have finished tagging the last image, close the **Image Detail** editor, and on the **Training Images** page, under **Tags**, select **Tagged** to see all of your tagged images:

      ![Picture1](../media/tagged-images-3b.png)

### Task 3: Train and Test a Model

Now that you have tagged the images in your project, you are ready to train a model.

1. In the Custom Vision project, click **Train** to train an object detection model using the tagged images. 

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg8.png)
   
1. Select the **Quick Training** option. Again, click **Train**.
 
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod3bimg9.png)

  
    > **Tip**: Training may take several minutes. While you are waiting, check out [Video analytics for smart-cities](https://www.microsoft.com/research/video/video-analytics-for-smart-cities/), which describes a real project to use computer vision in a road safety improvement initiative.

2. When training is complete, review the *precision*, *recall*, and *mAP* performance metrics. These measure the prediction goodness of the object detection model and should all be reasonably high.

3. Adjust the **Probability Threshold** on the left, increasing it from 50% to 90%, and observe the effect on the performance metrics. This setting determines the probability value that each tag evaluation must meet or exceed to be counted as a prediction.

      ![Screenshot of performance metrics for a trained model.](../media/performance-metrics-3b.png)

4. At the top right of the page, click **Quick Test**, and then in the **Image URL** box, enter `https://aka.ms/pedestrian-cyclist` and view the results.

      ![Screenshot of performance metrics for a trained model.](../media/quicktest-3b.png)
      
    In the right pane, under **Predictions**, each detected object is listed with its tag and probability. Select each object to see it highlighted in the image.

    The predicted objects may not all be correct - after all, cyclists and pedestrians share many common features. The predictions that the model is most confident about have the highest probability values. Use the **Threshold Value** slider to eliminate objects with a low probability. You should be able to find a point at which only correct predictions are included (probably at around 85-90%).

      ![Screenshot of performance metrics for a trained model.](../media/test-detection-3b.png)

5. Then, close the **Quick Test** window.

### Task 4: Publish the object detection model

Now, you are ready to publish your trained model and use it from a client application.

1. Click **&#128504; Publish**.

   ![Photograph of a group of pedestrians.](../media/ai900mod3bimg10.png)
 
1. To publish the trained model with the following settings:
    - **Model name**: traffic-safety **(1)**
    - **Prediction resource**: ****aiservice-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Click **Publish (3)**

1. After publishing, click the *Prediction URL* (&#127760;) icon to see the information required to use the published model.

      ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab2-2.png)
       
Later, you will need the appropriate URL and Prediction-Key values to get a prediction from an Image URL, so keep this dialog box open and carry on to the next task.

### Task 5: Prepare a client application

To test the capabilities of the Custom Vision service, we'll use a simple command-line application that runs in the cloud shell on Azure.

1. Switch back to the browser tab containing the Azure portal, and select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/powershell-portal-guide-1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/ai900mod1img6.png)

2. In the command shell, enter the following commands to download the files for this exercise and save them in a folder named **ai-900** (after removing that folder if it already exists)

    ```PowerShell
    rm -r ai-search -f
    ```
    ```PowerShell
    git clone https://github.com/CloudLabs-MOC/AI-900-AIFundamentals ai-search
    ```

3. After the files have been downloaded, enter the following commands to change to the **ai-900** directory and edit the code file for this exercise:

    ```PowerShell
    cd ai-search
    ```
    ```PowerShell
    code detect-objects.ps1
    ```

    Notice how this opens an editor like the one in the image below:

      ![Screenshot of the code editor in the cloud shell.](../media/code-editor-3b.png)

     > **Tip**: You can use the separator bar between the cloud shell command line and the code editor to resize the panes.

4. Don't worry too much about the details of the code. The important thing is that it starts with some code to specify the prediction URL and key for your Custom Vision model. You'll need to update these so that the rest of the code uses your model.

    Click on the **Prediction URL** to get the **Prediction URL (1)** and **Prediction key (2)** from the dialog box you left open in the browser tab for your Custom Vision project. You need the versions to be used *if you have an image URL*.

    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/azure-ai-search-lab2-4.png)

    Use these values to replace the **YOUR_PREDICTION_URL** and **YOUR_PREDICTION_KEY** placeholders in the code file.
    After pasting the Prediction URL and Prediction Key values, the first two lines of code should look similar to this:

    
     > $predictionUrl="https..."   
     > $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."


6. After making the changes to the variables in the code, press **CTRL+S** to save the file. Then press **CTRL+Q** to close the code editor.

### Task 6: Test the client application

Now, you can use the sample client application to detect cyclists and pedestrians in images.

1. In the PowerShell pane, enter the following command to run the code:

    ```PowerShell
    ./detect-objects.ps1 1
    ```

    This code uses your model to detect objects in the following image:

     ![Photograph of a pedestrian and a cyclist.](../media/create-object-detection-solution/road-safety-1.jpg)

1. Review the prediction, which lists any objects detected with a probability of 90% or more, along with the coordinates of a bounding box around their location.

1. Now, let's try another image. Run this command:

    ```PowerShell
    ./detect-objects.ps1 2
    ```

    This time, the following image is analyzed:

    ![Photograph of a group of pedestrians.](../media/create-object-detection-solution/road-safety-2.jpg)

 Hopefully, your object detection model did well-detecting pedestrians and cyclists in the test images.

   
   >**Note**: If you are not able to see the result in Powershell, then navigate back to the custom vision portal and go to the **predictions** tab. You see the details of the images as showed below:

   ![Photograph of a group of pedestrians.](../media/ai900mod3bimg12.png)


## Summary

In this lab, you have covered the following:
  
  - Created a Custom Vision project.
  - Added and tagged images.
  - Trained and tested a model.
  - Published the object detection model.
  - Prepared a client application.
  - Tested the client application.

## Learn more

This exercise shows only some of the capabilities of the Custom Vision service. To learn more about what you can do with this service, see the [Custom Vision page](https://learn.microsoft.com/en-us/azure/ai-services/custom-vision-service/).

### You have successfully completed the lab. Click on Next from the bottom right corner.
