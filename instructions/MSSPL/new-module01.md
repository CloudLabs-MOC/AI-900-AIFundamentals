# Automated Machine learning using AML
 
## Overall Estimated Duration: 4 Hours

### Overview

In this lab, you'll explore Azure Machine Learning's capabilities by creating a workspace, setting up compute resources, and creating a dataset. You'll run an automated machine learning job to train and identify the best model, then deploy it as a predictive service. Finally, you'll test the deployed service to ensure it delivers accurate results. This lab will demonstrate how Azure Machine Learning can streamline your workflow and enhance productivity.

### Objective

Understand how to create and deploy an Azure Machine Learning workspace, set up compute resources, and create datasets. Gain skills in running automated machine learning jobs, reviewing models, and deploying predictive services. By the end of this lab, you will be able to:

- *Create and Deploy an Azure Machine Learning Workspace*: Learn how to set up a centralized hub for managing machine learning projects, including creating compute resources and datasets.

- *Run and Review Automated Machine Learning Jobs*: Understand how to train multiple models, identify the best one, and review its performance.
Deploy and Test Predictive Services: Gain the ability to deploy models as predictive services and test them to ensure they deliver accurate results.

### Pre-requisites 

- Basic understanding of machine learning concepts and workflows.
- Basic knowledge of data preparation and processing techniques.

### Architecture

This architecture flow demonstrates how various Azure components work together to handle, process, analyze, and visualize data, providing a comprehensive and intelligent system tailored to business needs. You’ll start by creating an Azure Machine Learning workspace to manage resources, followed by provisioning compute resources for experiments. Next, you’ll create and register datasets, run automated machine learning jobs to identify the best model, and review model performance. The best model is then deployed as a predictive service, which is tested to ensure accuracy. This integrated approach showcases Azure’s AI and data analysis capabilities, enhancing productivity and delivering personalized experiences.

![Architectural Diagram](../media/GettingStarted/archd.png)

### Explanation of Components

- *Azure OpenAI*: This component provides access to advanced AI models from OpenAI, enabling natural language processing and other AI capabilities in applications.
- *Azure Machine Learning Workspace*: Central hub for managing machine learning resources like datasets, experiments, and models. It supports collaboration and tracks the entire ML lifecycle.
- *Compute Cluster*: A group of interconnected computers that distribute and speed up machine learning tasks. It scales dynamically based on workload.
- *Machine Learning Model*: The output of the ML process, representing learned patterns from data. It is used to make predictions or decisions based on new data.

## Getting Started with Lab

1. Once the environment is provisioned, a virtual machine (JumpVM) and lab guide will get loaded in your browser. Use this virtual machine throughout the workshop to perform the lab. You can see the number on the lab guide bottom area to switch to different exercises of the lab guide.

   ![Create storage by clicking confirm.](../media/GettingStarted/gspage01.png)   
          
1. To get the lab environment details, you can select the **Environment Details** tab. Additionally, the credentials will also be emailed to your email address provided during registration. You can also open the Lab Guide on separate and full window by selecting the **Split Window** from the lower right corner. Also, you can start, stop, and restart virtual machines from the **Resources** tab.

   ![Create storage by clicking confirm.](../media/GettingStarted/ai-900-gettingstarted-04.png)
   
## Login to Azure Portal
1. In the JumpVM, click on Azure portal shortcut of Microsoft Edge browser which is created on desktop.
   
   ![Create storage by clicking confirm.](../media/GettingStarted/gspage02.png)   
 
   >**Note**: On the Welcome to Microsoft Edge page, select  **Start without your data**  and on the help for importing Google browsing data page, select the  **Continue without this data**  button. Then, proceed to select  **Confirm and start browsing**  on the next page

1. On **Sign into Microsoft Azure** tab you will see login screen, in that enter following email/username and then click on **Next**. 
   * Email/Username: <inject key="AzureAdUserEmail"></inject>

     ![Create storage by clicking confirm.](../media/GettingStarted/ai-900-sign-1.png)
     
 1. Now enter the following password and click on **Sign in**.
    * Password: <inject key="AzureAdUserPassword"></inject>
    
     
      ![Create storage by clicking confirm.](../media/GettingStarted/ai-900-sign-2.png)

  1. On the Action Required pop-up click on Ask later.

        ![Create storage by clicking confirm.](../media/GettingStarted/ai-900-sign-2.png)
      
      
 1. If you see the pop-up **Stay Signed in?**, click No

 1. If you see the pop-up **You have free Azure Advisor recommendations!**, close the window to continue the lab.

 1. If a **Welcome to Microsoft Azure** popup window appears, click **Maybe Later** to skip the tour.
   
 1. Now you will see Azure Portal Dashboard.  

    ![MFA](../media/GettingStarted/MFA.png)

   > NOTE: Do not enable MFA, select Ask Later.
    
 9. Now, click on the **Next** from lower right corner to move on next page.

 ## Support Contact
The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

- Email Support: labs-support@spektrasystems.com
- Live Chat Support: https://cloudlabs.ai/labs-support

Now, click on Next from the lower right corner to move on to the next page.

![Next](../media/GettingStarted/next.png)
## Happy Learning!!
