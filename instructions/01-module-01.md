# Module 01: Explore Azure AI services

> **Important**
> **The Anomaly Detector lab has been deprecated and replaced by the update below.**

Azure AI services help users create AI applications with out-of-the-box and pre-built and customizable APIs and models. In this exercise you will take a look at one of the services, Azure AI Content Safety, in the Content Safety Studio. 

The Content Safety Studio enables you to explore how text and image content can be moderated. You can run tests on sample text or images and get a severity score ranging from safe to high for each category. In this lab exercise you will create a single-service resource in the Content Safety Studio and test its functionalities. 

> **Note**
> The goal of this exercise is to get a general sense of how Azure AI services are provisioned and used. Content Safety is used as an example, but you are not expected to gain a comprehensive knowledge of content safety in this exercise!

## Navigate the Content Safety Studio 
<img src ="https://github.com/CloudLabs-MOC/AI-900-AIFundamentals/assets/95687117/86fcbdb7-41b3-4652-88fe-2f293581798d">


1. Open the [Content Safety Studio](https://contentsafety.cognitive.azure.com?azure-portal=true). If you are not logged in, you will need to sign in. Select **Sign In** on the top right of the screen. Use the email and password associated with your Azure subscription to sign in. 

2. The Content Safety Studio is set up like many other studios for Azure AI services. On the menu at the top of the screen, click on the icon on the left of *Azure AI*. You will see a drop-down list of other studios designed for development with Azure AI services. You can click on the icon again to hide the list.

<img src ="https://github.com/CloudLabs-MOC/AI-900-AIFundamentals/assets/95687117/3806d9ef-1cb1-45b7-8b63-ae5d8226b27e">  

## Associate a resource with the studio 


Before using the studio, you need to associate an Azure AI services resource with the studio. Depending on the studio, you may find you need a specific single-service resource, or can use a general multi-service resource. In the case of the Content Safety Studio, you can use the service by creating a single-service *Content Safety* resource or *Azure AI services* general multi-service resource. In the steps below, we will create a single-service Content Safety resource. 

1. On the top right of the screen, click on the **Settings** icon. 

<img src ="https://github.com/CloudLabs-MOC/AI-900-AIFundamentals/assets/95687117/9ccc532e-6ab6-4ea9-854d-b3462de6a05b">  

2. On the **Settings** page, you will see a *Directory* tab and *Resource* tab. On the *Resource* tab, select **Create a new resource**. This takes you to page to create a resource in the Azure Portal.

> **Note**
> The *Directory* tab allows users to select different directories from which to create resources. You do not need to change its settings unless you wish to use a different directory. 

<img src ="https://github.com/CloudLabs-MOC/AI-900-AIFundamentals/assets/95687117/eed82272-c68d-4557-abeb-3723346b9e2d"> 

1. On the *Create Content Safety* page in the [Azure Portal](https://portal.azure.com?auzre-portal=true), you need to configure several details to create your resource. Configure it with the following settings:
    - **Subscription**: *Your Azure subscription*.
    - **Resource group**: *Select or create a resource group with a unique name*.
    - **Region**: *Choose any available region*.
    - **Name**: *Enter a unique name*.
    - **Pricing tier**: Free F0

1. Select **Review + Create** and review the configuration. Then select **Create**. The screen will indicate when the deployment is complete. 

*Congrats! You've just created, or provisioned, an Azure AI services resource. The one you provisioned in particular is a single-service Content Safety service resource.*

1. When the deployment is complete, open up a new tab and return to the [Content Safety Studio](https://contentsafety.cognitive.azure.com?azure-portal=true). 

2. Select the **Settings** icon on the top right of the screen again. This time you should see that your newly created resource has been added to the list.  

3. On the Content Safety Studio's Settings page, select the Azure AI service resource you just created and click **Use resource** on the bottom of the screen. You will be taken back to the studio home page. Now you can begin using the studio with your newly created resource.

## Try out text moderation in the Content Safety Studio

1. On the Content Safety Studio home page, under *Run moderation tests*, navigate to the **Moderate text content** box and click **Try it out**.
2. Under run a simple test, click **Safe Content**. Notice that text is displayed in the box below. 
3. Click **Run test**. Running a test calls the Content Safety Service's deep learning model. The deep learning model has already been trained to recognize un-safe content.
4. In the *Results* panel, inspect the results. There are four severity levels from safe to high, and four types of harmful content. Does the Content Safety AI service consider this sample to be acceptable or not? What's important to note is that the results are within a confidence interval. A well-trained model, like one of Azure AI's out-of-the-box models, can return results that have a high probability of matching what a human would label the result. Each time you run a test, you call the model again. 
5. Now try another sample. Select the text under Violent content with misspelling. Check that the content is displayed in the box below.
6. Click **Run test** and inspect the results in the Results panel again. 

You can run tests on all the samples provided, then inspect the results.

## Check out the keys and endpoint

These capabilities you tested can be programmed into all sorts of applications. The keys and endpoint used for application development can be found both in the Content Safety Studio and the Azure Portal. 

1. In the Content Safety Studio, navigate back to the **Settings** page, with the *Resources* tab selected. Look for the resource you used. Scroll across to see the endpoint and key for your resource. 
1. In the Azure Portal, you will see that these are the *same* endpoint and *different* keys for your resource. To check it out, head to the [Azure Portal](https://portal.azure.com?auzre-portal=true). Search for *Content Safety* on the top search bar. Find your resource and click on it. On the left-hand menu, look under *Resource Management* for *Keys and Endpoints*. Select **Keys and Endpoints** to view the endpoint and keys for your resource. 

After you have finished, you can delete the Content Safety resource from the Azure Portal. Deleting the resource is a way to reduce costs that accrue when the resource exists in the subscription. To do this, navigate to the **Overview** page of your Content Safety resource. Select **Delete** at the top of the screen. 
