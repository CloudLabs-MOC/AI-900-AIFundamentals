# Module 2: Explore Automated Machine Learning in Azure ML

In this exercise, you will use a dataset of historical bicycle rental details to train a model that predicts the number of bicycle rentals that should be expected on a given day, based on seasonal and meteorological features.

## Exercise 1: Create an Azure Machine Learning workspace  

### Task 1: Create an Azure Machine Learning workspace

1. Select **+ Create a resource**, search for Machine Learning.

    ![Picture1](media/ai900mod1img1.png)

1. In the Marketplace page search for **Azure Machine Learning** and Select **Azure Machine Learning**.
 
    ![Picture1](media/ai900mod2cimg1.png)

1. On **Azure Machine Learning** Page Click on **Create**.

   ![Picture1](media/ai900mod2cimg2.png)
   
1. create a new **Azure Machine Learning** resource with an *Azure Machine Learning* plan. Use the following settings:

    - **Subscription**: *Use the existing Azure subscription*
    - **Resource group**: Select **AI-900-Module-02-<inject key="DeploymentID" enableCopy="false"/>**
    - **Workspace name**: Enter **ai900workspace-<inject key="DeploymentID" enableCopy="false"/>**
    - **Region**: Select **<inject key="location" enableCopy="false"/>**
    - **Storage account**: *Note the default new storage account that will be created for your workspace*
    - **Key vault**: *Note the default new key vault that will be created for your workspace*
    - **Application insights**: *Note the default new application insights resource that will be created for your workspace*
    - **Container registry**: None (*one will be created automatically the first time you deploy a model to a container*)

1. Select **Review + create**, then select **Create**. Wait for your workspace to be created (it can take a few minutes), and then go to the deployed resource.

1. Select **Launch studio** (or open a new browser tab and navigate to [https://ml.azure.com](https://ml.azure.com?azure-portal=true), and sign into Azure Machine Learning studio using your Microsoft account).

1. Close any messages that are displayed.

1. In Azure Machine Learning studio, you should see your newly created workspace. If that is not the case, select your Azure directory in the left-hand menu. Then from the new left-hand menu select **Workspaces**, where all the workspaces associated to your directory are listed, and **select the one you created for this exercise**.

### Task 2: Create compute

1. In [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true), select the **&#8801;** icon (a menu icon that looks like a stack of three lines) at the top left to view the various pages in the interface (you may need to maximize the size of your screen). You can use these pages in the left hand pane to manage the resources in your workspace. Select **Compute**(under **Manage**).

1. On the **Compute** page, select the **Compute clusters** tab and to add a new compute cluster, click on **+ New** with the following settings. You'll use this to train a machine learning model:

      ![Picture1](media/ai900mod2cimg5.png)
      
    - **Location**: Select <inject key="location" enableCopy="false" />.
    - **Virtual machine tier**: Dedicated
    - **Virtual machine type**: CPU
    - **Virtual machine size**:
        - Choose **Select from all options**
        - Search for and select **Standard_DS11_v2**
    - Select **Next**
    
      ![Picture1](media/ai900mod2cimg6.png)
      
    - **Compute name**: Enter **ai900compute-<inject key="DeploymentID" enableCopy="false"/>**
    - **Minimum number of nodes**: 0
    - **Maximum number of nodes**: 2
    - **Idle seconds before scale down**: 120
    - **Enable SSH access**: keep it as default
    - Select **Create**

       ![Picture1](media/ai900mod2cimg7.png)
       
   > **Note**:The compute cluster will take some time to be created. You can move onto the next step while you wait.

### Task 3: Create a dataset

1. View the comma-separated data at [https://aka.ms/bike-rentals](https://aka.ms/bike-rentals?azure-portal=true) in your web browser.

1. In [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true), expand the left pane by selecting the menu icon at the top left of the screen. View the **Data** page (under **Assets**). The Data page contains specific data files or tables that you plan to work with in Azure ML. You can create datasets from this page as well.

1. On the **Data** page, under the **Data assets** tab, select **+ Create**. Then configure a data asset with the following settings:
    * **Data type**:
        * **Name**: bike-rentals
        * **Description**: Bicycle rental data
        * **Type**: Tabular
    * Click on **Next**.
    
    * **Data source**: From Web Files
    * * Click on **Next**.
    
    * **Web URL**:
        * **Web URL**: [https://aka.ms/bike-rentals](https://aka.ms/bike-rentals?azure-portal=true)
        * **Skip data validation**: *do not select*
    * Click on **Next**.
     
    * **Settings**:
        * **File format**: Delimited
        * **Delimiter**: Comma
        * **Encoding**: UTF-8
        * **Column headers**: Only first file has headers
        * **Skip rows**: None
        * **Dataset contains multi-line data**: *do not select*
    * Click on **Next**.

    * **Schema**:
        * Include all columns other than **Path**
        * Review the automatically detected types
    * Click on **Next**.

    * **Review**
        * Select **Create**

1. After the dataset has been created, open it and view the **Explore** page to see a sample of the data. This data contains historical features and labels for bike rentals.

> **Citation**: *This data is derived from [Capital Bikeshare](https://www.capitalbikeshare.com/system-data) and is used in accordance with the published data [license agreement](https://www.capitalbikeshare.com/data-license-agreement)*.

### Task 4: Run an automated machine learning job

Follow the next steps to run a job that uses automated machine learning to train a regression model that predicts bicycle rentals.

1. In [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true), expand the left pane by selecting the menu icon at the top left of the screen. View the **Automated ML** page (under **Authoring**).

1. Click on **+ New Automated ML job**. Create an Automated ML job with the following settings:

    - **Select data asset**:
        - **Dataset**: bike-rentals
    - Click on **Next**
    
    - **Configure job**:
        - **New experiment name**: mslearn-bike-rental
        - **Target column**: rentals(Integer) (*this is the label that the model is trained to predict)*
        - **Select compute type**: *Compute cluster*
        - **Select Azure ML compute cluster**: **ai900compute-<inject key="DeploymentID" enableCopy="false"/>**.
    - Click on **Next**.
    
    - **Select task and settings**: 
        - **Task type**: Regression *(the model predicts a numeric value)* 
   
     - Notice under task type there are settings *View additional configuration settings* and *View featurization settings*. Now configure these settings. Click on **View additional configuration settings**.

       ![Screenshot of a selection pane with boxes around the Regression task type and additional configuration settings.](media/use-automated-machine-learning/ai-900-regression.png)

    - **Additional configuration settings:**
        - **Primary metric**: Select **Normalized root mean squared error**
        - **Explain best model**: Selected — *this option causes automated machine learning to calculate feature importance for the best model which makes it possible to determine the influence of each feature on the predicted label.*
        - **Use all supported models**: <u>Un</u>selected. *You'll restrict the job to try only a few specific algorithms.*
        - **Allowed models**: *Select only **RandomForest** and **LightGBM** — normally you'd want to try as many as possible, but each model added increases the time it takes to run the job.*
        - **Exit criterion**:
            - **Training job time (hours)**: 0.5 — *ends the job after a maximum of 30 minutes.*
            - **Metric score threshold**: 0.085 — *if a model achieves a normalized root mean squared error metric score of 0.085 or less, the job ends.*
        - **Concurrency**: *do not change*
    - Click on **Save**.
  
      ![Screenshot of additional configurations with a box around the allowed models.](media/use-automated-machine-learning/ai-900-add-cong01.png)
      
    - Now click on **View featurization settings:**
        - **Enable featurization**: Selected — *automatically preprocess the features before training.*
    - Click on **Save**.

    - Click **Next** to go to the next selection pane.

    - **Select the validation and test type**
        - **Validation type**: Auto
        - **Test data asset (preview)**: No test data asset required
    - Click on **Finish**.

1. When you finish submitting the automated machine learning job details, it starts automatically. Wait for the status to change from *Preparing* to *Running*.

1. When the status changes to *Running*, view the **Models** tab and observe as each possible combination of training algorithm and pre-processing steps is tried and the performance of the resulting model is evaluated. The page automatically refreshes periodically, but you can also select **Refresh**. It might take 10 minutes or so before models start to appear, as the cluster nodes must be initialized before training can begin.

      ![Picture1](media/ai900lab2img3.png)

1. Wait for the job to finish. It might take a while — now might be a good time for a coffee break!

### Task 5: Review the best model

1. On the **Overview** tab of the automated machine learning job, note the best model summary.
    ![Screenshot of the best model summary of the automated machine learning job with a box around the algorithm name.](media/use-automated-machine-learning/ai-900-overview.png)

    >**NOTE:**
    > You may see a message under the status "Warning: User specified exit score reached...". This is an expected message. Please continue to the next step.  
1. Select the text under **Algorithm name** for the best model to view its details.

1. Next to the *Normalized root mean squared error* value, select **View all other metrics** to see values of other possible evaluation metrics for a regression model.

    ![Screenshot of how to locate view all other metrics on the Model tab.](media/use-automated-machine-learning/ai-900-overview-02.png)

1. Select the **Metrics** tab, use the arrows icon to expand the panel if it is not already expanded and select the **residuals** and **predicted_true** charts if they are not already selected. 

    ![Screenshot of the metrics tab with the residuals and predicted_true charts selected.](media/use-automated-machine-learning/ai-900-matrix1.png)

    Scroll down and review the charts which show the performance of the model. The first chart shows the *residuals*, the differences between predicted and actual values, as a histogram, the second chart compares the predicted values against the true values.

1. Select the **Explanations(preview)** tab. Select an Explanation ID and then select **Aggregate feature importance** tab. This chart shows how much each feature in the dataset influences the label prediction, like this:

    ![Screenshot of the feature importance chart on the Explanations tab.](media/use-automated-machine-learning/feature-importance1.png)

### Task 6: Deploy a predictive service

1. In [Azure Machine Learning studio](https://ml.azure.com?azure-portal=true), on the **Automated ML** page, select your automated machine learning job.

1. On the **Overview** tab, select the algorithm name for the best model.

    ![Screenshot of the best model summary with a box around the algorithm name on the details tab.](media/use-automated-machine-learning/ai-900-algorithm.png)

1. On the **Model** tab, select the **Deploy** button and use the **web service** option.

      ![Picture1](media/ai900lab2img2.png)
      
3. To deploy the model with the following settings and then click on **Deploy**.
    - **Name**: predict-rentals
    - **Description**: Predict cycle rentals
    - **Compute type**: Azure Container Instance
    - **Enable authentication**: Selected

       ![Picture1](media/ai900lab2img1.png)

1. Wait for the deployment to start - this may take a few seconds. Then, in the **Model summary** section, observe the **Deploy status** for the **predict-rentals** service, which should be **Running**. Wait for this status to change to **Succeeded**, which may take some time. You may need to select **Refresh** periodically.

1. In Azure Machine Learning studio, on the left hand menu, select **Endpoints**.

    ![Screenshot of location of Endpoints on the left hand menu.](media/endpoints1-02.png)

### Task 7: Test the deployed service

Now you can test your deployed service.

1. On the **Endpoints** page, open the **predict-rentals** real-time endpoint.

    ![Screenshot of location of Endpoints on the left hand menu.](media/use-automated-machine-learning/endpoints-2.png)

> **Note**: The realtime endpoint may be in unhealthy state, wait for another 30 minutes for the endpoint state to change the deployment state to **Healthy**, or else perform the steps from Task 5.

### Learn more

**Azure Machine Learning Endpoints** provide an improved, simpler deployment experience. To learn more about what you can do with this service, see the [Understanding_Service_State](https://learn.microsoft.com/en-us/azure/machine-learning/v1/how-to-deploy-and-where?tabs=azcli&view=azureml-api-1#understanding-service-state).

1. When the **predict-rentals** endpoint opens, view the **Test** tab.

1. In the **Input data to test real-time endpoint** pane, replace the template JSON with the following input data:

    ```JSON
    {
      "Inputs": { 
        "data": [
          {
            "day": 1,
            "mnth": 1,   
            "year": 2022,
            "season": 2,
            "holiday": 0,
            "weekday": 1,
            "workingday": 1,
            "weathersit": 2, 
            "temp": 0.3, 
            "atemp": 0.3,
            "hum": 0.3,
            "windspeed": 0.3 
          }
        ]    
      },   
      "GlobalParameters": 1.0
    }
    ```

1. Click on the **Test** button.

1. Review the test results, which include a predicted number of rentals based on the input features. The test pane took the input data and used the model you trained to return the predicted number of rentals.

    ![Screenshot of an example of testing the model with sample data in the test tab.](media/use-automated-machine-learning/workaround-test1.png)

Let's review what you have done. You used a dataset of historical bicycle rental data to train a model. The model predicts the number of bicycle rentals expected on a given day, based on seasonal and meteorological *features*. In this case, the *labels* are number of bicycle rentals.

You have just tested a service that is ready to be connected to a client application using the credentials in the **Consume** tab. We will end the lab here. You are welcome to continue to experiment with the service you just deployed.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Click Lab Validation tab located at the upper right corner of the lab guide section and navigate to the Lab Validation Page.
   > - Hit the Validate button for the corresponding task.  
   > - If you receive a success message, you can proceed to the next task.If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.

### You have successfully completed this lab.
