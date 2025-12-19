# Lab 01: Detect and Analyze Faces

## Estimated Duration: 90 Minutes

## Overview

The ability to detect and analyze human faces is a core AI capability. In this exercise, you'll explore two Azure Cognitive Services that you can use to work with faces in images: the **Computer Vision** service and the **Face** service.

## Objectives

In this lab, you will complete the following tasks:

+ **Task 1:** Open the cloned folder in Visual Studio Code.
+ **Task 2:** Provision a Cognitive Services resource
+ **Task 3:** Prepare to use the Azure AI Vision SDK
+ **Task 4:** View the image you will analyze
+ **Task 5:** Detect faces in an image
+ **Task 6:** Prepare to use the Face SDK
+ **Task 7:** Detect and analyze faces

## Architecture diagram

![](./images/mod10-ex1-dia.png)

## Task 1: Open the cloned folder in Visual Studio Code

In this task, you will open the cloned project folder in Visual Studio Code to access and modify the necessary files for the lab. This setup ensures you can edit, run, and test the code efficiently.

1. In the Lab-VM desktop, double-click on the **Visual Studio Code**.

    ![Visual Studio Code Icon](./images/vscode(1).png)

1. Open a file, from the top-left options, click on **Explorer (1)-> Open Folder (2)** and navigate to **C:\AllFiles (3)**, choose **AI-102-AIEngineer-stage (4)** folder and click **Select folder (5)**.

    ![](./images/botframe1(6).png)

    >**Note:** Do you trust the authors of the files in this folder? prompt, select **Yes, I trust the authors**.

3.  Wait while additional files are installed to support the C# code projects in the repo.

## Task 2: Provision a Cognitive Services resource

In this task, you will provision an Azure AI Services resource in the Azure portal. This resource will provide the necessary cognitive capabilities for your application. You will configure the resource with appropriate settings, retrieve the endpoint and key, and prepare it for integration with your project.

1. Open the Azure portal at `https://portal.azure.com`, and sign in using the Microsoft account associated with your Azure subscription.

2. Select the **&#65291;Create a resource** button.

    ![](./images/detana(1).png)

1. Search for **Azure AI Services (1)** and press **Enter**, then select **Azure AI services (2)** from the results. 

    ![](./images/detana(2).png)

1. On the Azure AI services blade, click on **Create**.

    ![](./images/detana(3).png)

1. On the Create Azure AI service blade, create a resource with the following settings:
    
    - **Subscription**: *Your Azure subscription* **(1)**
    - **Resource group**: **Ai-102-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - **Region**: **<inject key="Region" enableCopy="false"/> (3)**
    - **Name**: **aicognitive<inject key="DeploymentID" enableCopy="false"/>** **(4)**
    - **Pricing tier**: Standard S0 **(5)**
    - Select the required checkbox **(6)**
    - Click on **Review + create (7)**

        ![](./images/detana(4).png)

4. Navigate to the **Review + create** tab, and click **Create**.

    ![](./images/detana(5).png)

5. Wait for deployment to complete, and then click **Go to resource**.

    ![](./images/detana(6).png)

6. In the **Azure AI Services** blade, under **Resource Management (1)**, click **Keys and Endpoint (2)**. From this page, you’ll need the **Endpoint (3)** and one of the keys (**KEY 1 (4)**) for the next step.

    ![](./images/detana(7).png)

## Task 3: Prepare to use the Azure AI Vision SDK

In this task, you'll complete a partially implemented client application that uses the Azure AI Vision SDK to analyze faces in an image.

1. In Visual Studio Code, in the **Explorer** pane, browse to the **19-face (1)** folder and expand the **C-Sharp (2)** folder depending on your language preference. Right-click the **computer-vision (3)** folder and select **Open in Integrated Terminal (4)**. 

    ![](./images/detana(8).png)

1. Then install the Computer Vision SDK package by running the following command:

    **C#**

    ```
    dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 6.0.0
    ```
        
3. View the contents of the **computer-vision** folder, and note that it contains a file for configuration settings:
   
    - **C#**: appsettings.json

4. Open the configuration file and update the configuration values it contains to reflect the **endpoint** and an authentication **key** for your cognitive services resource. Save your changes.

    ![](./images/detana(9).png)

5. Note that the **computer-vision** folder contains a code file for the client application:

    - **C#**: Program.cs

6. Open the code file and at the top, under the existing namespace references, find the comment **Import namespaces**. Then, under this comment, add the following code to import the namespaces you will need to use the Computer Vision SDK and save the file:

    **C#**

    ```C#
    // import namespaces
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
    ```

    ![](./images/detana(10).png)

## Task 4: View the image you will analyze

In this exercise, you will use the Computer Vision service to analyze an image of people.

1. In Visual Studio Code, expand the **computer-vision** folder and the **images** folder it contains.

2. Select the **people.jpg** image to view it.

    ![](./images/detana(11).png)

## Task 5: Detect faces in an image

In this task, you will use the SDK to call the Computer Vision service and detect faces in an image.

1. In the code file for your client application (**Program.cs**), in the **Main** function, note that the code to load the configuration settings has been provided. Then find the comment **Authenticate Computer Vision client**. Then, under this comment, add the following code to create and authenticate a Computer Vision client object:

    **C#**

    ```C#
    // Authenticate Azure AI Vision client
    ApiKeyServiceClientCredentials credentials = new ApiKeyServiceClientCredentials(cogSvcKey);
    cvClient = new ComputerVisionClient(credentials)
    {
        Endpoint = cogSvcEndpoint
    };
    ```

    ![](./images/detana(12).png)

2. In the **Main** function, under the code you just added, note that the code specifies the path to an image file and then passes the image path to a function named **AnalyzeFaces**. This function is not yet fully implemented.

3. In the **AnalyzeFaces** function, under the comment **Specify features to be retrieved (faces)**, add the following code:

    **C#**

    ```C#
    // Specify features to be retrieved (faces)
    List<VisualFeatureTypes?> features = new List<VisualFeatureTypes?>()
    {
        VisualFeatureTypes.Faces
    };
    ```

    ![](./images/detana(13).png)

4. In the **AnalyzeFaces** function, under the comment **Get image analysis**, add the following code:

     **C#**

    ```C
     // Get image analysis
     using (var imageData = File.OpenRead(imageFile))
     {    
         var analysis = await cvClient.AnalyzeImageInStreamAsync(imageData, features);

         // Get faces
         if (analysis.Faces.Count > 0)
         {
             Console.WriteLine($"{analysis.Faces.Count} faces detected.");

         // Prepare image for drawing
         Image image = Image.FromFile(imageFile);
         Graphics graphics = Graphics.FromImage(image);
         Pen pen = new Pen(Color.LightGreen, 3);
         Font font = new Font("Arial", 3);
         SolidBrush brush = new SolidBrush(Color.LightGreen);

         // Draw and annotate each face
         foreach (var face in analysis.Faces)
         {
            var r = face.FaceRectangle;
            Rectangle rect = new Rectangle(r.Left, r.Top, r.Width, r.Height);
            graphics.DrawRectangle(pen, rect);
            string annotation = $"Person at approximately {r.Left}, {r.Top}";
            graphics.DrawString(annotation,font,brush,r.Left, r.Top);
         }

         // Save annotated image
         String output_file = "detected_faces.jpg";
         image.Save(output_file);
         Console.WriteLine(" Results saved in " + output_file);   
       }
   }        
    ```

6. Save your changes and return to the integrated terminal for the **computer-vision** folder, and enter the following command to run the program:

    **C#**

    ```
    dotnet run
    ```

7. Observe the output, which should indicate the number of faces detected.

    ![](./images/detana(14).png)

8. View the **detected_faces.jpg** file that is generated in the same folder as your code file to see the annotated faces. In this case, your code has used the attributes of the face to label the location of the top left of the box, and the bounding box coordinates to draw a rectangle around each face.

    ![](./images/detana(15).png)

## Task 6: Prepare to use the Face SDK

In this task, you will prepare to use the Face SDK in your Visual Studio Code project. You will install the required package, update the configuration settings, and authenticate the Face client. Additionally, you will import the necessary namespaces and ensure that your application is ready to interact with the Azure Face service for facial recognition and analysis.

1. In Visual Studio Code, in the **Explorer** pane, browse to the **19-face** folder and expand the **C-Sharp** folder.Right-click the **face-api (1)** folder and select **Open in Integrated Terminal (2)**. 

    ![](./images/detana(16).png)

1. Then install the Face SDK package by running the following command:

    **C#**

    ```
    dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.6.0-preview.1
    ```
    
3. View the contents of the **face-api** folder, and note that it contains a file for configuration settings:
   
    - **C#**: appsettings.json

4. Open the configuration file and update the configuration values it contains to reflect the **endpoint** and an authentication **key** for your cognitive services resource. Save your changes.

    ![](./images/detana(17).png)

5. Note that the **face-api** folder contains a code file for the client application:

    - **C#**: Program.cs

6. Open the code file and at the top, under the existing namespace references, find the comment **Import namespaces**. Then, under this comment, add the code to import the namespaces you will need to use the Computer Vision SDK:

    **C#**

    ```C#
    // Import namespaces
    using Microsoft.Azure.CognitiveServices.Vision.Face;
    using Microsoft.Azure.CognitiveServices.Vision.Face.Models;
    ```

    ![](./images/detana(18).png)

8. In the **Main** function, note that the code to load the configuration settings has been provided. Then find the comment **Authenticate Face client**. Then, under this comment, add the following code to create and authenticate a **FaceClient** object:

    **C#**

    ```C#
    // Authenticate Face client
    ApiKeyServiceClientCredentials credentials = new ApiKeyServiceClientCredentials(cogSvcKey);
    faceClient = new FaceClient(credentials)
    {
        Endpoint = cogSvcEndpoint
    };
    ```

    ![](./images/detana(19).png)

9. In the **Main** function, under the code you just added, note that the code displays a menu that enables you to call functions in your code to explore the capabilities of the Face service. You will implement these functions in the remainder of this exercise.

## Task 7: Detect and analyze faces

In this task, you'll update the **DetectFaces** function to analyze images using the Azure Face SDK. You'll detect faces, retrieve attributes like occlusion, blur, and glasses, and annotate the image with bounding boxes. Finally, you'll run the program and review the detected faces.

1. In the code file for your application, in the **Main** function, examine the code that runs if the user selects menu option **1**. This code calls the **DetectFaces** function, passing the path to an image file.
   
3. Find the **DetectFaces** function in the code file, and under the comment **Specify facial features to be retrieved**, add the following code:

    **C#**

    ```C#
    // Specify facial features to be retrieved
    List<FaceAttributeType?> features = new List<FaceAttributeType?>
    {
        FaceAttributeType.Occlusion,
        FaceAttributeType.Blur,
        FaceAttributeType.Glasses
    };
    ```

    ![](./images/detana(20).png)

4. In the **DetectFaces** function, under the code you just added, find the comment **Get faces** and add the following code:

     **C#**

     ```C
     // Get faces
     using (var imageData = File.OpenRead(imageFile))
     {    
         var detected_faces = await faceClient.Face.DetectWithStreamAsync(imageData, returnFaceAttributes: features, returnFaceId: false);

         if (detected_faces.Count > 0)
         {
             Console.WriteLine($"{detected_faces.Count} faces detected.");

             // Prepare image for drawing
             Image image = Image.FromFile(imageFile);
             Graphics graphics = Graphics.FromImage(image);
             Pen pen = new Pen(Color.LightGreen, 3);
             Font font = new Font("Arial", 4);
             SolidBrush brush = new SolidBrush(Color.Black);
             int faceCount=0;

             // Draw and annotate each face
             foreach (var face in detected_faces)
             {
                 faceCount++;
                 Console.WriteLine($"\nFace number {faceCount}");
            
                 // Get face properties
                 Console.WriteLine($" - Mouth Occluded: {face.FaceAttributes.Occlusion.MouthOccluded}");
                 Console.WriteLine($" - Eye Occluded: {face.FaceAttributes.Occlusion.EyeOccluded}");
                 Console.WriteLine($" - Blur: {face.FaceAttributes.Blur.BlurLevel}");
                 Console.WriteLine($" - Glasses: {face.FaceAttributes.Glasses}");

                 // Draw and annotate face
                 var r = face.FaceRectangle;
                 Rectangle rect = new Rectangle(r.Left, r.Top, r.Width, r.Height);
                 graphics.DrawRectangle(pen, rect);
                 string annotation = $"Face ID: {face.FaceId}";
                 graphics.DrawString(annotation,font,brush,r.Left, r.Top);
             }

             // Save annotated image
             String output_file = "detected_faces.jpg";
             image.Save(output_file);
             Console.WriteLine(" Results saved in " + output_file);   
         }
     }
     ```

4. Examine the code you added to the **DetectFaces** function. It analyzes an image file and detects any faces it contains, including attributes for age, emotions, and the presence of spectacles. The details of each face are displayed, including a unique face identifier that is assigned to each face, and the location of the faces is indicated on the image using a bounding box.

5. Save your changes and return to the integrated terminal for the **face-api** folder, and enter the following command to run the program:

    **C#**

    ```
    dotnet run
    ```

    >**Note:** The C# output may display warnings about asynchronous functions now using the **await** operator. You can ignore these.

6. When prompted, enter **1** and observe the output, which should include the ID and attributes of each face detected.

    ![](./images/detana(21).png)

7. View the **detected_faces.jpg** file that is generated in the same folder as your code file to see the annotated faces.

    ![](./images/detana(22).png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
>
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

<validation step="8b14af8e-6a29-4e7e-a695-8261457d1a46" />

## Summary
In this lab, you have completed:

+ Opened the cloned folder in Visual Studio Code.
+ Provisioned a Cognitive Services resource
+ Prepared to use the Azure AI Vision SDK
+ Viewed the image you will analyze
+ Detected faces in an image
+ Prepared to use the Face SDK
+ Detected and analyzed faces
   
### You have successfully completed the Hands-on lab!