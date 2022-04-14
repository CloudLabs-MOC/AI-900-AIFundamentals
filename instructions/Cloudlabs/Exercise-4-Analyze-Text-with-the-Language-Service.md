# Analyze Text with the Language Service

In this lab, you will use the Language cognitive service to analyze text.​

## Exercise 1 : Analyze text

Natural Language Processing (NLP) is a branch of artificial intelligence (AI) that deals with written and spoken language. You can use NLP to build solutions that extracting semantic meaning from text or speech, or that formulate meaningful responses in natural language.

Microsoft Azure  _Cognitive Services_  includes the text analytics capabilities in the  _Language_  service, which provides some out-of-the-box NLP capabilities, including the identification of key phrases in text, and the classification of text based on sentiment.

For example, suppose the fictional  _Margie's Travel_  organization encourages customers to submit reviews for hotel stays. You could use the Language service to summarize the reviews by extracting key phrases, determine which reviews are positive and which are negative, or analyze the review text for mentions of known entities such as locations or people.

To test the capabilities of the Language service, we'll use a simple command-line application that runs in the Cloud Shell. The same principles and functionality apply in real-world solutions, such as web sites or phone apps.

### Task 1 : Create a  _Cognitive Services_  resource

You can use the Language service by creating either a  **Language**  resource or a  **Cognitive Services**  resource.

If you haven't already done so, create a  **Cognitive Services**  resource in your Azure subscription.

1.  In another browser tab, open the Azure portal at  [https://portal.azure.com](https://portal.azure.com/), signing in with your Microsoft account.
    
2.  Select the  **＋Create a resource**  button, search for  _Cognitive Services_, and create a  **Cognitive Services**  resource with the following settings:
    
    -   **Subscription**:  _Your Azure subscription_.
    -   **Resource group**:  _Select existing resource group_.
    -   **Region**:  _Choose any available region_:
    -   **Name**:  cognitive-[DeploymentID].
    -   **Pricing tier**: S0
    -   **I confirm I have read and understood the notices**: Selected.

  **Note**:  Replace [DeploymentID] with your DeploymentId, which can be obtained from the Lab Environment Details page.
  
    
3.  Review and create the resource.
    

### Task 2 : Get the Key and Location for your Cognitive Services resource

1.  Wait for deployment to complete. Then go to your Cognitive Services resource, and on the  **Overview**  page, select the link to manage the keys for the service. You will need the endpoint and keys to connect to your Cognitive Services resource from client applications.
    
2.  View the  **Keys and Endpoint**  page for your resource. You will need the  **location/region**  and  **key**  to connect from client applications.
    

### Task 3 : Run Cloud Shell

To test the text analytics capabilities of the Language service, we'll use a simple command-line application that runs in the Cloud Shell on Azure.

1.  In the Azure portal, select the  **[>_]**  (_Cloud Shell_) button at the top of the page to the right of the search box. This opens a Cloud Shell pane at the bottom of the portal.
    
    ![Start Cloud Shell by clicking on the icon to the right of the top search box](../media/powershell-portal-guide-1.png)
    
2.  The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (_Bash_  or  _PowerShell_). Select  **PowerShell**. If you do not see this option, skip the step.
    
3.  When prompted, select **Show advanced settings** and then select **Use existing** and choose existing resource group. Then select **Create new** against Storage account as well as File Share and provide a unique value in both of the fields and then click on **Create storage**, and wait for the Azure Cloud Shell to initialize.

    ![Create storage by clicking confirm.](../media/Ai-900p1.png)
    
4.  Make sure the type of shell indicated on the top left of the Cloud Shell pane is switched to  _PowerShell_. If it is  _Bash_, switch to  _PowerShell_  by using the drop-down menu.
    
    ![How to find the left hand drop down menu to switch to PowerShell](../media/powershell-portal-guide-3.png)
    
5.  Wait for PowerShell to start. You should see the following screen in the Azure portal:
    
    ![Wait for PowerShell to start.](../media/powershell-prompt.png)
    

### Task 4 : Configure and run a client application

Now that you have a custom model, you can run a simple client application that uses the Language service.

1.  In the command shell, enter the following command to download the sample application and save it to a folder called ai-900.
    

    ```
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    
    ```
    
2.  The files are downloaded to a folder named  **ai-900**. Now we want to see all of the files in your Cloud Shell storage and work with them. Type the following command into the shell:
    

    ```
    code .
    
    ```
    
    Notice how this opens up an editor like the one in the image below:
    
    ![The code editor.](../media/powershell-portal-guide-4.png)
    
3.  In the  **Files**  pane on the left, expand  **ai-900**  and select  **analyze-text.ps1**. This file contains some code that uses the Language service:
    
    ![The editor containing code to use the Language service](../media/analyze-text-code.png)
    
4.  Don't worry too much about the details of the code. In the Azure portal, navigate to your Cognitive Services resource. Then select the  **Keys and Endpoints**  page on the left hand pane. Copy the key and endpoint from the page and paste them into the code editor, replacing the  **YOUR_KEY**  and  **YOUR_ENDPOINT**  placeholder values respectively.
    
    Tip
    
    You may need to use the separator bar to adjust the screen area as you work with the  **Keys and Endpoint**  and  **Editor**  panes.
    
    ![Find the key and endpoint tab in your Cognitive Services resource's left hand pane.](../media/key-endpoint-support.png)
    
    After replacing the key and endpoint values, the first lines of code should look similar to this:
    
    PowerShellCopy
    
    ```
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    
    ```
    
5.  At the top right of the editor pane, use the  **...**  button to open the menu and select  **Save**  to save your changes. Then open the menu again and select  **Close Editor**.
    
    The sample client application will use Cognitive Services' Language service to detect language, extract key phrases, determine sentiment, and extract known entities for reviews.
    
6.  In the Cloud Shell, enter the following command to run the code:
    

    ```
    cd ai-900
    ./analyze-text.ps1 review1.txt 
    
    ```
    
    You will be reviewing this text:
    
    > Good Hotel and staff The Royal Hotel, London, UK 3/2/2018 Clean rooms, good service, great location near Buckingham Palace and Westminster Abbey, and so on. We thoroughly enjoyed our stay. The courtyard is very peaceful and we went to a restaurant which is part of the same group and is Indian ( West coast so plenty of fish) with a Michelin Star. We had the taster menu which was fabulous. The rooms were very well appointed with a kitchen, lounge, bedroom and enormous bathroom. Thoroughly recommended.
    
7.  Review the output.
    
8.  In the PowerShell pane, enter the following command to run the code:
    
    ```
    ./analyze-text.ps1 review2.txt 
    
    ```
    
    
   
    
    You will be reviewing this text:
    
    > Tired hotel with poor service The Royal Hotel, London, United Kingdom 5/6/2018 This is a old hotel (has been around since 1950's) and the room furnishings are average - becoming a bit old now and require changing. The internet didn't work and had to come to one of their office rooms to check in for my flight home. The website says it's close to the British Museum, but it's too far to walk.
    
    
9.   Review the output.
    
10.  In the PowerShell pane, enter the following command to run the code:


      ```
      ./analyze-text.ps1 review3.txt 
    
      ```
    
    
   
    
    
    
   
    
   You will be reviewing this text:
    
   > Good location and helpful staff, but on a busy road. The Lombard Hotel, San Francisco, USA 8/16/2018 We stayed here in August after reading reviews. We were very pleased with location, just behind Chestnut Street, a cosmopolitan and trendy area with plenty of restaurants to choose from. The Marina district was lovely to wander through, very interesting houses. Make sure to walk to the San Francisco Museum of Fine Arts and the Marina to get a good view of Golden Gate bridge and the city. On a bus route and easy to get into centre. Rooms were clean with plenty of room and staff were friendly and helpful. The only down side was the noise from Lombard Street so ask to have a room furthest away from traffic noise.
   
    
    
11.  Review the output.
    
12.  In the PowerShell pane, enter the following command to run the code:
    
    
   
     ```
     ./analyze-text.ps1 review4.txt
    
     ```
    
   
   
   
   You will be reviewing this text:
    
   > Very noisy and rooms are tiny The Lombard Hotel, San Francisco, USA 9/5/2018 Hotel is located on Lombard street which is a very busy SIX lane street directly off the Golden Gate Bridge. Traffic from early morning until late at night especially on weekends. Noise would not be so bad if rooms were better insulated but they are not. Had to put cotton balls in my ears to be able to sleep--was too tired to enjoy the city the next day. Rooms are TINY. I picked the room because it had two queen size beds--but the room barely had space to fit them. With family of four in the room it was tight. With all that said, rooms are clean and they've made an effort to update them. The hotel is in Marina district with lots of good places to eat, within walking distance to Presidio. May be good hotel for young stay-up-late adults on a budget
    
13.  Review the output.
    

## Learn more

This simple app shows only some of the capabilities of the Language service. To learn more about what you can do with this service, see the  [Language service page](https://azure.microsoft.com/services/cognitive-services/language-service/).
