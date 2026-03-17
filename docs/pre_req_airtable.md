# Airtable Base

## Introduction to Data Repositories: Airtable 

[Airtable](https://airtable.com/) is a cloud-based platform that blends the intuitive interface of a spreadsheet with the advanced capabilities of a relational database. It allows users to create **Bases**—highly customizable databases—to organize, manage, and collaborate on complex data sets. For our purposes, Airtable provides a robust **REST API** that enables the AI Agent to programmatically access and manipulate data in real-time.

During this bootcamp, you will implement a financial service use case for **Webex Bank**. Airtable serves as our centralized *"Headless Database"*, providing the backend logic required to power the AI Agent’s decision-making. We will establish three core tables to simulate a production-grade enterprise environment:

- **Customers**: For identity verification, profile management and debt status.
- **Transactions**: For retrieving recent account activity and resolving disputes.
- **Investments**: For providing real-time portfolio updates and financial advice.

Rather than just manual data entry, you will leverage the **Airtable API** to transform static records into dynamic conversational triggers. This allows your AI Agent to perform sophisticated logic—such as verifying a caller's identity or fetching a 7-day transaction history—effectively bridging the gap between AI orchestration and external data repositories.

Each Airtable base includes interactive, auto-generated API documentation tailored to its specific structure. For the scope of these labs:

- Authentication: Access is secured via **Personal Access Tokens** (PAT).
- Capacity: The Airtable Free Plan supports up to 1,000 records and 1GB of storage per base.
- API Limits: The workspace supports up to 1,000 API calls per month, which is ample for our lab exercises.

The following sections will guide you through constructing your Airtable base and optionally configuring Postman to validate that your API collection is ready for integration.

???+ tip
    Airtable is a versatile asset for customer demonstrations, enabling you to build bespoke datasets that align perfectly with a client's specific use case. By integrating this data, you can demonstrate highly personalized Webex Contact Center (WxCC) flows that not only retrieve information in real-time but also dynamically update the database based on customer interactions. With its ready-to-use API, Airtable allows you to visualize and prove the power of a data-driven customer experience with minimal configuration.

---

##  Build your Airtable base

Let´s start building your Airtable Base

???+ Webex "Create your free account in Airtable"

    **You can skip this step if you have already an account**.

    Got to <https://www.airtable.com/> and click on "Sign up for free".

    ???+ inline end "Create Airtable account"

        <figure markdown>
        ![Create Airtable Account](./assets/airtable1.png)
        </figure>

    Complete the steps to create your new account. You can skip creating your first app with the Assistant by clicking on
    the "x" at the upper-right corner of the screen and move to create a blank app. 
    
    After all this process, you will land into a brand new empty Base. A **Base** is the fundamental organizational unit---think of it as a database for a specific project or purpose. A Base is a container that holds all the tables, fields, records, views, and automations related to a single project or workflow. It\'s similar to a spreadsheet
    workbook or a relational database, but with a more visual and user-friendly interface.
    
    ???+ inline end "Create Airtable Base"

        <figure markdown>
        ![Create Airtable Base](./assets/airtable2.png)
        </figure>

    Before building our Base, we will setup some other entities in Airtable. Click on the upper-left Airtable logo to "go home".

???+ Webex "Create your first Workspace"

    ???+ inline end "Create Workspace"
         <figure markdown>
        ![Create Workspace](./assets/airtable3.png)
        </figure>
    
    Click on the "+" sign besides the "All workspaces"
    
    In the next window, give your Workspace a name. Say "WxCC Bootcamp".
    Then click on **[+ Create]** and in the next dialogue select **Build an app on your own**. Your are now ready to create your Base. 
    

???+ Webex "Create your Bootcamp Base"

    ???+ gif inline end "Create your Base"
        <figure markdown>
        ![Create Base](./assets/airtable6.gif)
        </figure>

    
    Let´s build our base. After the previous step, you will land into an empty grid view similar to the one below:


    First, give it a name, changing the "Untitled Base" into a meaningful name: "Webex Bank". For that, click in the "Untitle Base" and provide the new name. 
   
    Next we will need to create the three tables for our Webex Bank: **Customers**, **Transactions** and **Investments**. We will proceed the same way for them: 

    1. Import a csv file with the table template
    2. Rename the table
    2. Change the field types to the appropiate ones (the import converts all field types into *long string*)

    ???+ Webex "***Customers*** table"

        !!! download "Customers Table"
            Download the csv file for the [Customers Table](./bcamp_files/airtable_Customers.csv).

        1. Once you have downloaded your .csv file, click on **+ Add or import** and select **CSV file**. Drop or select your `airtable_Customers.csv` file and click **[Upload 1 file]**

        2. In the **Import your file** dialogue, select *+ Create a new table* and click **[Next]**

        2. In the next window, you will need to adjust the type of some of the fields. Click the chevron next to the field name to expand the properties menu, then select the correct field type for the following fields: 

            |   Field   | Type       |
            | :----     | :----     |
            | CustomerID | `Single line text` |
            | PIN      | `Single line text` |
            | DOB       |   `Date`  |
            | Balance   |   `Currency`|
            | MaturityDate | `Date`  |
            | CashbackBalance |  `Currency`|
            | Email | `Email` |
            | RewardsTier | `Single select`|
            | CreditCard    | `Single select`|

            Now, click **[Import]**

        3. Change the name of the Imported table to **Customers**. Click on the chevron `v` besides the **Imported table** table name  and select *Rename table*. Type **Customers** and click **[Save]**

        4. Make sure the Date type fields have a proper date configuration (**DOB** and **MaturityDate**). Click  the chevron in the field, select **Edit field** and under **Date format** select *Local* and make sure the **include time* option is deactivated. Click **[Save]*

        4. Now populate the values for the *Single select* field types: 

            - Click the chevron besides the **RewardsTier** field, click **Edit field** and then **+ Add option** to add the following options: 
                
                    
                <br><copy>Basic</copy>
                <br><copy>Standard</copy>
                <br><copy>Elite</copy>
                    
                Select *Basic* as the **Default** value and click **[Save]**

            - Do the same for the **CreditCard** field with the following values: 

                <br><copy>Standard Cashback</copy>
                <br><copy>Travel Rewards Card</copy>
                <br><copy>Platinum Card</copy>

                Select *Standard Cashback* as the **Default** value and click **[Save]** 

        Your **Customers** table is ready.     
        
    ???+ Webex "***Transactions*** table"

        !!! download "Transactions Table"
            Download the csv file for the [Transactions Table](./bcamp_files/airtable_Transactions.csv).

        Now proceed as in the **Customers** table to import the csv file of the **Transactions** table and adjust the field types for the following fields: 

        |   Field   | Type       |
        | :----     | :----     |
        | Amount      | `Currency`  |
        | Vendor       |   `Single line text`  |
        | City       |   `Single line text`  |
        | Date   |   `Date`|
        
        Click **[Import]**
        
        Change the name of the Imported table to **Transactions**: Click on the chevron besides the table name *Imported table* and select *Rename table*. Type **Transactions** and click **[Save]**
        
        We still need to adjust a few fields: 

        - In the  **TransactionID** field, click the chevron, select **Edit field** and change the type to *Autonumber*. This will automatically generate a transaction ID to every record you create.
        - In the **CustomerID** field, click the chevron, select **Edit field** and change the type to *Link to another record*. From the existing tables list, select **Customers** and click **[Save]**.
            - In the next dialogue window, select the fields we want to import from the **Customers** table: **FirstName** and **LastName**. Click **[Add 2 fields]**. This will create two new columns in your table.
        This will keep both tables connected and consistent. You will see in your Customers table a new field called **Transactions**. it will contain links to all the transactions associated to the customer. 
       
    ???+ Webex "***Investments*** table"

        !!! download "Investments Table"
            Download the csv file for the [Investments Table](./bcamp_files/airtable_Investments.csv).


    Your Airtable Base is now ready for the Bootcamp!!! Let´s now setup the API that will allow you to access your data from your flows.
---

## Setting up your Airtable API

Airtable's API uses token-based authentication, allowing users to
authenticate API requests by inputting their tokens into the HTTP
authorization bearer token header. Two type of tokens are supported:
personal access tokens and OAuth access tokens. As this is for a demo
environment, we will use [Personal Access
Tokens](https://airtable.com/developers/web/guides/personal-access-tokens)
(PAT).

???+ Webex "Create a Personal Access Token"

    1. To create your PAT, go to <https://airtable.com/create/tokens> and click
    on "Create token".

    ???+ gif inline end "Create your Token"
        <figure markdown>
        ![Create Token](./assets/airtable7.gif)
        </figure>

    2. Give your token a name: "Webex Bank PAT".

    3. Define the Scope for your token. You will need to select **data.records:read** and **data.records:write**, as we will need R/W access to the data.

    4. Click **+ Add a base** and select your workspace to enable API access to your database.

    5. Click on "Create Token"

    6. You should see a confirmation panel with your newly created token. This   will be the only time you will see your token, it will never be shown again, so make sure you copy it in a safe place for further use. Once saved, you can click on "Done".

    Now you will see your new token in your Personal access token site:

???+ Webex "APi Documentation"

    ???+ inline end "Create your Token"
        <figure markdown>
        ![Create Token](./assets/airtable16.png)
        </figure>
    
    Let's now generate the API documentation for your Airtable Base:

    1.  Go to <https://airtable.com/api>. This is Airtable's official API documentation portal.

    2.  Log into your Airtable account (if you're not already logged in).

    3.  You'll see the list of **Bases** you have access to. Click on your **Base**.

    
    4.  Airtable will generate **auto-documented API reference** for that Base, which includes:

        - Authentication setup

        - Endpoints for each **Table**

        - Example **GET**, **POST**, **PATCH**, and **DELETE** requests

        - Field names and data types

        - Sample code snippets (e.g., using curl, JavaScript, etc.)

    ???+ inline end "API Documentation"
        <figure markdown>
        ![API Documentation](./assets/airtable17.png)
        </figure>


    ???+ Tip
        Make sure you have populated at least one entry in your tables so that the API documentation includes examples.

    Bookmark this page as it will be your API reference for your base.

    As you can see on the documentation, The **Airtable API URL** to access a base's content is constructed using a specific format that includes your **Base ID**, **Table Name (or ID)**, and the **API version**:

        https://api.airtable.com/v0/{{baseId}}/{{tableId}}

    - {{baseId}} -- A unique identifier for your base (e.g. app1234567890abcdef).

    - {{tableId}} -- The unique identity of the table. (e.g., tblABCDsGD34DS49J2). The tableId can be replaced by the
    {{tableName}}. If the table name contains spaces or special characters, it should be **URL-encoded** (e.g., My%20Table). To avoid formatting issues, we recommend you to use {{tableld}}

    ???+ inline end "API URL components"
        <figure markdown>
        ![API URL](./assets/airtable18.png)
        </figure>
    
    Where you can find this information? This is detailed in the generated API documentation for your base, but it is also available in your base page. See the picture on the right.

    Take note of those values.

---

???+ Challenge "Test your Airtable API with Postman"

    you can create records, modify information and visualize the data in your Airtable Base through the Airtable web interface. So strictly speaking, you would not need to define an Airtable API collection in Postman for the Bootcamp labs. But this collection in Postman would allow you to test and verify that your Airtable API is working fine, as we will use the API from our flows to read and write data in the Base. So feel free to setup your Airtable API collection in Postman and test your Base to get it fully ready for the Bootcamp. 