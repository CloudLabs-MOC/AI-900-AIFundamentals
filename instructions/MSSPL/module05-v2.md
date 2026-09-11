# Hands-on Lab: Explore Knowledge Mining

### Estimated time: 120 minutes

## 📘 Scenario

Fourth Coffee, a coffee chain with stores across the country, has a growing pile of customer feedback - online reviews, scanned comment cards from in-store kiosks, and photos customers snap of their drinks or the storefront. Nobody on the team has time to read through all of it by hand, so patterns like which store gets complained about most, which phrase keeps showing up in the negative reviews, or whether customers are happier this quarter than last, stay hidden. You've been brought in to fix that: build a knowledge mining solution on **Azure AI Search** that reads every review the moment it lands, automatically tags it with the location it mentions, the key phrases inside it, its overall sentiment, and even a description of any attached photo - then makes all of that instantly searchable.

## 📖 Overview

This lab walks you through building that solution end to end. You'll spin up the three Azure resources it depends on, load a batch of Fourth Coffee's review documents into Blob Storage, and use Azure AI Search's **Import data** wizard to turn them into an AI-enriched index - no code required for most of it. A couple of pieces (scoring sentiment, and saving a full copy of the enriched data to a knowledge store) have fallen out of the wizard's checkbox list in recent portal updates, so you'll patch those back in yourself by editing the generated skillset directly - a normal thing to do on a real Azure AI Search project, and good practice for working with the service beyond its point-and-click surface. You'll wrap up by running searches and filters against your finished index, and browsing the parallel copy of the data sitting in your knowledge store.

## 🎯 Objective

In this lab, you will complete the following exercises:

- **Exercise 1:** Create the Azure resources

- **Exercise 2:** Upload Documents and Build the Search Index

- **Exercise 3:** Extend the Skillset for Sentiment Analysis and a Knowledge Store

- **Exercise 4:** Query the Index

- **Exercise 5:** Review the Knowledge Store

## Exercise 1: Create the Azure Resources

In this exercise, you will provision the three Azure resources this lab depends on: an Azure AI Search resource to hold your index, a Foundry resource to power the AI enrichments, and a Storage account to hold your documents. Each one only takes a few minutes to deploy, and you will create them one at a time across three tasks.

### Task 1: Create an Azure AI Search Resource

1. In the **Azure portal**, in the search box, type **ai Search (1)** and select **AI Search(Foundry IQ) (2)**.

    ![](../media/knowledge-mining/LTS111.png)

1. On the **Microsoft Foundry | AI Search** page, select **+ Create**.

    ![](../media/knowledge-mining/LTS112.png)

1. On the **Create a search service**, 

    - Subscription: Leave the default one **(1)**
    - Resource group: Select **AzureAI-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Service name: Enter **aisearch<inject key="DeploymentID" enableCopy="false"/> (3)**
    - Location: Select **<inject key="Region" enableCopy="false" /> (4)**
    - Pricing Tier: Select **Change Pricing Tier (5)** 

      ![](../media/knowledge-mining/LTS113.png)

    - Select **Free (1)** and then **Select (2)**. (If **Free** is not available or is already in use, choose **Basic**)

      ![](../media/knowledge-mining/LTS114.png)   

    - Select **Review+Create**

      ![](../media/knowledge-mining/LTS115.png)   

1. Select **Create**.

    ![](../media/knowledge-mining/LTS116.png)

    > **Note:** If AI Search service deployment fails due to regional capacity, try another supported region such as **Canada Central, East US, West US 2, Australia East, or Sweden Central**.

1. Once the deployment is complete, select **Go to resource**.

    ![](../media/knowledge-mining/LTS117.png)

### Task 2: Create a Storage Account

1. From the Azure portal, search for **Storage account (1)** and then select **Storage account (2)**.

    ![](../media/knowledge-mining/LTS131.png)

1. On the **Storage Center | Blob Storage** page, Select **Create**.

    ![](../media/knowledge-mining/LTS132.png)

1. On the **Create a storage account** page, provide the following details:

    - Subscription: Leave the default one **(1)**
    - Resource group: Select **AzureAI-<inject key="DeploymentID" enableCopy="false"/> (2)**
    - Storage account name: Enter **mystorage<inject key="DeploymentID" enableCopy="false"/> (3)**
    - Region: Select **<inject key="Region" enableCopy="false" /> (4)**
    - Primary service: **Azure Blob Storage or Azure Data Lake Storage (5)**
    - Performance: **Standard (6)**
    - Redundancy: Select **Locally-redundant storage(LRS) (7)**
    - Select **Review + Create (8)**

      ![](../media/knowledge-mining/LTS133.png)

1. Then **Create** to deploy the storage account.

    ![](../media/knowledge-mining/LTS134.png)

1. Once deployment is complete, click **Go to resource** to navigate to your new storage account.

    ![](../media/knowledge-mining/LTS135.png)

## Exercise 2: Upload Documents and Build the Search Index

In this exercise, you will upload the Fourth Coffee review documents to Blob Storage, then run the Import data wizard to turn them into a searchable, AI-enriched index. The wizard walks you through connecting to your data, choosing which AI skills to run, and shaping the resulting index - each as its own task below.

### Task 1: Upload Documents to Azure Storage

1. In the left-hand menu of the storage account blade, under **Data storage (1)**, click **Containers (2)**. On the Containers page, click **+ Add container (3)**.

    ![](../media/knowledge-mining/LTS136.png)

1. On the **New container** page,

    - Name: Enter **coffee-reviews (1)**
    - Anonymous access level: Leave Public access level as **Private (2)**
    - Select **Create (3)** 

    ![](../media/knowledge-mining/LTS211.png)

1. Select **coffee-reviews** container.

    ![](../media/knowledge-mining/LTS212.png)

1. Open a new browser tab, and paste the below link to download the zipped **reviews.zip (1)** file. Click on **Open (2)**

    ```
    https://aka.ms/km-documents
    ```

    ![](../media/knowledge-mining/LTS137.png)

1. **Right click (1)** on **reviews.zip** and click on **Extract all (2)** (unzip) to get all the files extracted from zip file

    ![](../media/knowledge-mining/LTS138.png)

1. Switch back to the **Azure portal** browser tab. Click **Upload** in the container toolbar to upload the document files into the container.

    ![](../media/knowledge-mining/LTS213.png)

1. Click **Browse for files(1)**, navigate to `C:\Users\azureuser\Downloads\reviews` **(2)**. Select all the document files **(3)** and then **Open (4)**.

    ![](../media/knowledge-mining/LTS140.png)

1. Select **Upload** to upload all the files into the container.    

    ![](../media/knowledge-mining/LTS214.png)

1. After uploading, the files should appear in the container’s file list. Ensure you see the blob name and its size in the list.

    ![](../media/knowledge-mining/LTS215.png)

### Task 2: Start the Import Data Wizard

Once you have the documents in storage, you can use Azure AI Search to extract insights from them. The Azure portal provides an **Import data** wizard, which walks you through creating a data source, a skillset, an index, and an indexer - all in one guided flow.

1. In the **Azure portal**, in the search box, type **ai Search (1)** and select **AI Search(Foundry IQ) (2)**.

    ![](../media/knowledge-mining/LTS111.png)

1. Select **aisearch<inject key="DeploymentID" enableCopy="false"/>**

    ![](../media/knowledge-mining/LTS221.png)

1. On the **Overview** page, select **Import data**.

    ![](../media/knowledge-mining/LTS222.png)

1. Select **Azure Blob Storage** as the data source.

    ![](../media/knowledge-mining/LTS223.png)

1. On the scenario picker screen, select the **Keyword search** tile.

    ![](../media/knowledge-mining/LTS224.png)

    > **Note :** The wizard also offers **RAG** and **Multimodal RAG** tiles - those are for building chatbot-style search over vectorized content, which isn't what this lab covers. **Keyword search** is the classic, AI-enriched full-text search this lab needs.

1. On the **Connect to your data** page, provide the following details:

    - **Subscription:** Select your subscription from the dropdown **(1)**
    - **Storage account:** Select **mystorage<inject key="DeploymentID" enableCopy="false"/> (2)**
    - **Blob container:** Select **coffee-reviews (3)**
    - **Parsing mode:** Leave it as **Default (4)**
    - Select **Next (5)**.

        ![](../media/knowledge-mining/LTS225.png)

    >**Note**: It may 2-3 minutes to go to the **Apply AI enrichments** page

1. On the **Apply AI enrichments** page, select the **Extract entities (1)** tile then  select its **settings (2)** (gear) icon on that tile.

    ![](../media/knowledge-mining/LTS226.png)

1. In the window that opens, select the **Locations (1)** checkbox and click **Save (2)**

    ![](../media/knowledge-mining/LTS227.png)

    >**Note:** If the **Persons** or **Organizations** checkboxes are already selected, clear them - you only need locations for this lab.

1. Select the **Extract key phrases** tile. This will produce a `keyphrases` field in your index.

    ![](../media/knowledge-mining/LTS228.png)

1. Select the **Extract text from images (1)** tile. This turns on OCR, so any text inside images and scanned files becomes searchable. On that tile, select its **settings (2)** (gear) icon.

    ![](../media/knowledge-mining/LTS229.png)

1. In the window that opens, select the **Generate tags (1)** checkbox and select **Save (2)**. This will produce an `imageTags` field in your index

    ![](../media/knowledge-mining/LTS230.png)

    > **Note :** Because you selected both OCR and image analysis, the wizard automatically adds a **Text Merge** skill behind the scenes. It combines each document's text with its OCR and image-analysis output into one field named **merged_content**. That's the field location extraction, key phrase extraction, and (later) sentiment analysis will all read from.

1. Review all the three options are selected and click on **Next**.

    ![](../media/knowledge-mining/LTS231.png)

1. On the **Preview mappings** page, confirm that the **Key** is set to **metadata_storage_path** and then click on **Next**. This field uniquely identifies each document, so Azure AI Search uses it as the document's key.

    ![](../media/knowledge-mining/LTS232.png)

1. On the **Advanced settings** page, **uncheck** the **semantic ranking** option. Leave the remaining as default and Select **Next**. 

    ![](../media/knowledge-mining/LTS233.png)

1. On the **Review and create** page, find the **Object name prefix** box and change it to **coffee-reviews (1)**. Review the summary and then select **Create (2)**.

    ![](../media/knowledge-mining/LTS234.png)

1. Once **Create Succeeded**, click **Go to Search explorer**.

    ![](../media/knowledge-mining/LTS235.png)

1. Select **aisearch<inject key="DeploymentID" enableCopy="false"/>** from the breadcrumbs.

    ![](../media/knowledge-mining/LTS237.png)

    >**Note:** You would see a message "The indexer associated with this index is in progress." Please wait a few minutes and navigate to the next step.

1. Under **Search management (1)**, select **Indexes (2)**. Keep **refresh (3)** until  the index **coffee-reviews** shows `9` documents **(4)**. Once it does, select option **(5)**.

    ![](../media/knowledge-mining/LTS236.png)

    >**Note:** It may take a few minutes for the count to reflect 9 documents. If the count appears incorrect, please refresh the entire tab and verify again.

## Exercise 3: Extend the Skillset for Sentiment Analysis and a Knowledge Store

In this exercise, you will extend the skillset the wizard just built, since two features - sentiment scoring and a full knowledge store - are no longer available as wizard checkboxes. You'll edit the skillset's JSON directly to add them back, then update the index and indexer to match.

### Task 1: Add the Sentiment Skill and a Shaper Skill and a Knowledge store

1. From the Azure portal, search for **Storage account (1)** and then select **Storage account (2)**.

    ![](../media/knowledge-mining/LTS131.png)

1. Select **mystorage<inject key="DeploymentID" enableCopy="false"/>**

    ![](../media/knowledge-mining/LTS311.png)

1. In the left-hand menu, under **Security + networking (1)**, select **Access keys (2)**. Under **key1**, find the **Connection string** box. Click on **Show (3)** and copy the connection string **(4)**and paste it in notepad.

    ![](../media/knowledge-mining/LTS312.png)

1. Go back to your **Azure AI Search** resource.

1. In the left-hand navigation pane, expand **Search management (1)**, then select **Skillsets (2)**. Select **coffee-reviews-skillset (3)** from the list.

    ![](../media/knowledge-mining/LTS313.png)

1. In the JSON text, place the cursor at the end of line 4, immediately after `"skills": [`. Press **Enter** to add a new line, and paste the provided skill definition before the existing skill entries.

    ```
    {
        "@odata.type": "#Microsoft.Skills.Text.V3.SentimentSkill",
        "context": "/document",
        "inputs": [
            { "name": "text", "source": "/document/merged_content" }
        ],
        "outputs": [
            { "name": "sentiment", "targetName": "sentiment" }
        ]
    },
    {
        "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
        "context": "/document",
        "inputs": [
            { "name": "metadata_storage_name", "source": "/document/metadata_storage_name" },
            { "name": "metadata_storage_path", "source": "/document/metadata_storage_path" },
            { "name": "merged_content", "source": "/document/merged_content" },
            { "name": "locations", "source": "/document/merged_content/locations" },
            { "name": "keyphrases", "source": "/document/merged_content/keyPhrases" },
            { "name": "sentiment", "source": "/document/sentiment" },
            { "name": "imageTags", "source": "/document/normalized_images/*/tags/*/name" }
        ],
        "outputs": [
            { "name": "output", "targetName": "kstoreDoc" }
        ]
    },
    ```

    ![](../media/knowledge-mining/LTS314.png)

    > **What did you just add?** The **Sentiment** skill reads the `merged_content` field and labels it positive, neutral, or negative. The **Shaper** skill doesn't analyze anything - it just gathers up all the enriched fields you care about (locations, key phrases, sentiment, and image tags) into one bundle, so the knowledge store in the next task has a single, tidy object to save per document. `imageTags` pulls the `name` out of every tag object across every image in the document into one flat list - if you also selected **Generate captions** when you configured the **Extract text from images** skill, you can add a ninth input, `{ "name": "imageCaption", "source": "/document/normalized_images/*/captions/*/text" }`, following the same pattern.

1. In the same JSON editor, scroll to the end of the file, find the closing `]` of the **skills** section, place the cursor immediately after it and before the final `}`, type a comma `,`, press **Enter**, and paste the provided text.

    ```
    "knowledgeStore": {
        "storageConnectionString": "<paste your storage account connection string here>",
        "projections": [
            {
                "tables": [
                    { "tableName": "coffeeSkillsetDocuments", "generatedKeyName": "DocumentId", "source": "/document/kstoreDoc" },
                    { "tableName": "coffeeSkillsetKeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/kstoreDoc/keyphrases/*" }
                ],
                "objects": [],
                "files": []
            },
            {
                "tables": [],
                "objects": [
                    { "storageContainer": "knowledge-store", "source": "/document/kstoreDoc" }
                ],
                "files": [
                    { "storageContainer": "coffee-skillset-image-projection", "source": "/document/normalized_images/*" }
                ]
            }
        ]
    }
    ```

    > **Why two groups?** Each projection *group* (each `{ ... }` inside the `projections` array) can't have two selectors pointing at the same `source`/`sourceContext` - Azure AI Search rejects the skillset with an error like `Multiple projection selectors have source/sourceContext '...'` if it finds a duplicate. Splitting the table projections into one group and the object/file projections into a second group avoids that error, even though both groups read from the same `/document/kstoreDoc` bundle.

    ![](../media/knowledge-mining/LTS315.png)

1. Replace `<paste your storage account connection string here>` with the connection string **(1)** you copied in Task 1 of this exercise. Select **Save (2)** to save the changes in JSON editor.


    ![](../media/knowledge-mining/LTS316.png)

    >**Note:** If you see a red error message, it almost always means a missing or extra comma - compare your brackets and commas carefully against the snippets above and try again.

### Task 2: Add and Map Sentiment Field

1. Go back to your **Azure AI Search** resource. In the left-hand navigation pane, select **Search management**, then select **Indexes**. Select **coffee-reviews**.

    ![](../media/knowledge-mining/LTS316.png)

1. Select the **Fields (1)** tab then select **+ Add field (2)**.

1. A new window open in right side. Provide the below details and select **Save (6)**

    -  **Field name:** Enter **sentiment** in the box **(3)**.
    - **Type:** Select **Edm.String** from the dropdown **(4)**.
    - **Field options:** Select the **Retrievable**, **Filterable**, and **Facetable** checkboxes **(5)**.

    ![](../media/knowledge-mining/LTS317.png)

1. Select **Save** to save the changes

    ![](../media/knowledge-mining/LTS318.png)

1. Go back to your **Azure AI Search** resource. In the left-hand navigation pane, select **Search management (1)**, then select **Indexers (2)**. Select **coffee-review-indexer (3)**

    ![](../media/knowledge-mining/LTS320.png)

1. Open its **Edit JSON** or code view, the same way you did for the skillset.

    ![](../media/knowledge-mining/LTS319.png)

1. In the **outputFieldMappings** section, place the cursor immediately after `"outputFieldMappings": [`. Press **Enter** to add a new line. Add the following entry after the existing mapping entries **(1)** and click on **Save(2)**

    ```json
    {
    "sourceFieldName": "/document/sentiment",
    "targetFieldName": "sentiment"
    },
    ```

    ![](../media/knowledge-mining/LTS321.png)

1. Now select **Reset** and click on **Yes** on the popup. This clears the indexer's memory of what it already processed, so it will reprocess every document from scratch through your updated skillset.

    ![](../media/knowledge-mining/LTS322.png)

1. Once reset is completed, select **Run (1)** click on **Yes** on the popup to start the indexer again.

    ![](../media/knowledge-mining/LTS323.png)

    >**Note:** Select **&orarr; Refresh (2)** every minute or so, and wait until the **Status** column shows **Success (3)** before moving on to the next exercise.

## Exercise 4: Query the Index

In this exercise, you will use **Search explorer**, a query tool built into the Azure portal, to test your finished index. You'll count documents, then filter them by location and by sentiment to see the enriched fields in action.

### Task 1: Explore, Analyze, and Filter Search Results

1. In your AI Search service's **Overview** page, select **Search explorer** at the top of the screen.

    ![](../media/knowledge-mining/LTS411.png)

1. In the query box, type `*`, and select **Search**. This returns every document in the index. In the results, look for the `@odata.count` field near the top - this shows the total number of matching documents.

    ![](../media/knowledge-mining/LTS412.png)

1. Near the query box, select **View (1)** (or the toggle) and switch to **JSON view (2)**.

    ![](../media/knowledge-mining/LTS413.png)

1. In the JSON view box, replace the query with below **(1)**. Select **Search (2)**. This filters for reviews that mention a Chicago location.

    ```json
    {
        "search": "*",
        "filter": "locations/any(l: l eq 'Chicago')",
        "count": true
    }
    ```

    ![](../media/knowledge-mining/LTS414.png)

    > **Note :** `locations` is a collection (an array of strings), so the filter needs the `any()` lambda operator instead of a plain `eq` comparison - `any(l: l eq 'Chicago')` reads as "any single location value in this list equals Chicago."

1. In the JSON view box, replace the query with below **(1)**. Select **Search (2)**. This filters for reviews with a negative overall sentiment.

    ```json
    {
        "search": "*",
        "filter": "sentiment eq 'negative'",
        "count": true
    }
    ```

    ![](../media/knowledge-mining/LTS415.png)

    > **Note :** See how the results are sorted by `@search.score`. This is the score assigned by the search engine to show how closely the results match the given query.

1. Scroll through a few of the negative results, and look at each document's `keyPhrases` and `content` fields.

    ![](../media/knowledge-mining/LTS416.png)

## Exercise 5: Review the Knowledge Store

In this exercise, you will browse the knowledge store you configured in Exercise 3 - the parallel copy of your enriched data saved as tables, JSON objects, and images in your storage account, ready for other tools to consume.

### Task 1: Review the Object, Image and Table Projections

1. From the Azure portal, search for **Storage account (1)** and then select **Storage account (2)**.

    ![](../media/knowledge-mining/LTS131.png)

1. Select **mystorage<inject key="DeploymentID" enableCopy="false"/>**

    ![](../media/knowledge-mining/LTS511.png)

1. In the left-hand menu, under **Data storage (1)**, select **Containers (2)**. Select the **knowledge-store (3)** container.

    ![](../media/knowledge-mining/LTS512.png)

1. Select any **folder** in the list.

    ![](../media/knowledge-mining/LTS520.png)

1. Select the file **(1)** inside the folder. Each one is named after its source document and ends in **.json**. Select **Edit (2)** to view the JSON produced for that document - including its locations, key phrases, sentiment, and image tags **(3)**

    ![](../media/knowledge-mining/LTS513.png)

1. Select the storage account breadcrumb link at the top-left of the screen to return to **Containers**.

    ![](../media/knowledge-mining/LTS514.png)

1. From the **Containers** list, select the **coffee-skillset-image-projection** container.

    ![](../media/knowledge-mining/LTS515.png)

1. Select any **folder** in the list.

    ![](../media/knowledge-mining/LTS516.png)

1. Select any of the **.jpg (1)** files in the list. Select **Edit (2)** to view the image. Notice how all the images from the documents are stored here, individually **(3)**.

    ![](../media/knowledge-mining/LTS517.png)

1. Select the storage account breadcrumb link at the top-left of the screen again to return to **Containers**.

    ![](../media/knowledge-mining/LTS519.png)

1. Select **Storage browser (1)** on the left-hand panel, and select **Tables (2)**. There's a table for each entity in the index. Select the table **coffeeSkillsetKeyPhrases (3)**.

    ![](../media/knowledge-mining/LTS518.png)

    Look at the key phrases the knowledge store was able to capture from the content in the reviews. Many of the fields are keys, which is what lets you link the tables together like a relational database. The last field shows the key phrases that were extracted by the skillset.

## 🧾 Summary

In this lab, you created the Azure resources behind a knowledge mining solution - an Azure AI Search resource, a Foundry resource, and a Storage account. You uploaded a set of customer review documents and used the Import data wizard to build an AI-enriched search index that extracts locations, key phrases, image tags, and image captions. You then extended the generated skillset by hand to add sentiment analysis and a full knowledge store, since those are no longer available as wizard checkboxes. Finally, you queried your index in Search explorer and browsed the enriched tables, objects, and images saved to your knowledge store.

### You have successfully completed this lab.

## Learn more

This simple search index only shows some of the capabilities of Azure AI Search. To learn more about what you can do with this service, see the [Azure AI Search documentation](https://learn.microsoft.com/azure/search/) and the [Import data wizard reference](https://learn.microsoft.com/azure/search/search-import-data-portal).
