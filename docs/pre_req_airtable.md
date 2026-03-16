## Airtable 

[Airtable](https://airtable.com/) is a cloud-based platform that
combines the features of a spreadsheet with the power of a relational
database. It allows users to create **bases**---customizable
databases---for organizing, managing, and collaborating on data.
Airtable provides a robust REST API that enables developers to
programmatically access and manipulate data in any base. Uses personal
access tokens or OAuth for secure API access. Each base has
automatically generated, interactive API documentation tailored to its
structure. The Airtable free plan allows unlimited bases with up to 1000
records each and up to 1GB storage per base. It does support up to 1000
API calls per workspace each month.

During the Bootcamp you will be implementing an Order Management use
case for an online company. In the context of the Bootcamp, we will use
Airtable to simulate the interactions of a production database
containing the orders and customer information. Here we describe how you
can build your Airtable base with the required data and setup the API
that you will need to use it to implement HTTP data requests in the
flows.

NOTE: This can be a shared resource for all your attendees in the
Bootcamp. Every attendee would need to create its own customer record
for their labs if you are not working as a team . That will be described
in lab 1. In this document we will cover the construction of your
Airtable base and the required setup in Postman to test and make sure
the Airtable base API collection is ready for the Bootcamp.

***This is a very useful tool for you to use with your customer demos,
as it allows you to build customer data that can be easily customized to
your customers´ use cases and leveraged to personalize your WxCC flows
as well as to visualize how a flow can also modify the data. It also
provides a ready-to-use API to access the data from your flows.***

###  Build your Airtable base

1.  **Create your free account in Airtable**.

> You can skip this step if you have already an account.
>
> Got to <https://www.airtable.com/> and click on "Sign up for free".
>
> ![A screenshot of a web page AI-generated content may be
> incorrect.](./images/media/image1.png){width="5.995386045494313in"
> height="3.0151235783027124in"}
>
> Complete the steps to create your new account. You can skip adding
> coworkers. Also you can skip creating your first app by clicking on
> the "x" at the upper-right corner of the screen. You will also get a
> 14-day trial of a Team plan. No payment is required and it will
> rollback to the free plan after 14 days.
>
> After all this process, you will land into a brand new empty Base. A
> **Base** is the fundamental organizational unit---think of it as a
> database for a specific project or purpose. A Base is a container that
> holds all the tables, fields, records, views, and automations related
> to a single project or workflow. It\'s similar to a spreadsheet
> workbook or a relational database, but with a more visual and
> user-friendly interface.
>
> Before building our Base, we will setup some other entities in
> Airtable. Click on the upper-left Airtable logo close to the "Untitled
> Base" name to "go home".

Click here to go to the home page

2.  **Create your first Workspace**.

> Click on the "+" sign besides the "All workspaces"
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image3.png){width="4.601804461942257in"
> height="3.109851268591426in"}
>
> In the next window, give your Workspace a name. Say "WxCC Partner
> Bootcamp".
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image4.png){width="5.206747594050744in"
> height="2.811574803149606in"}

3.  **Create your Bootcamp Base**.

> Click on "Start from scratch" box to create your base. You will get
> into this empty Grid view:
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image5.png){width="6.361731189851269in"
> height="3.4352504374453194in"}
>
> Let´s build our base. Give it first a name, changing the "Untitled
> Base" into a meaningful name: "WxCC Bootcamp".
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image6.png){width="2.1665879265091865in"
> height="2.9603805774278213in"}
>
> Now, rename your Table into "Order_CRM". This will be our customer and
> order data repository for the Bootcamp. You can also give a name to
> every record in the table: "Order". Save it.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image7.png){width="2.1693022747156605in"
> height="3.047020997375328in"} ![A screenshot of a computer
> AI-generated content may be
> incorrect.](./images/media/image8.png){width="1.7706113298337707in"
> height="2.550323709536308in"}

4.  **Build the structure of your Database**.

> We will now define the fields for our database. For that we will need
> to change the existing fields an add new ones. To change a field,
> click on the "v" symbol in the field header and "Edit field". To add a
> new one, simply click on the "+" symbol at the right of the last
> field.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image9.png){width="6.270287620297463in"
> height="3.0123206474190725in"}
>
> Let´s modify the first one as an example. Click on the "v" symbol on
> the right of the field "name" and then click "Edit field". In the next
> panel, give it the name "orderId" and select the "Single line text"
> field type. This will be our primary field in the table. Save it to
> get your first field.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image10.png){width="3.0435772090988626in"
> height="2.9634831583552055in"}
>
> Proceed in the same way, modifying or creating new fields until you
> complete the below table structure.
>
> NOTE: we recommend you to use the proposed names below as it will make
> easy to keep up with the lab documents during the bootcamp.

  ----------------------------------------
  Field name         Field type
  ------------------ ---------------------
  orderId            Single line text

  consentComs        Single line text

  fName              Single line text

  lName              Single line text

  phoneNumber        Single line text

  email              Email

  address            Single line text

  timezone           Single line text

  productName        Single line text

  deliveryAddress    Single line text

  deliveryETA        Date -\> Local

  altDate1           Date -\> Local

  altDate2           Date -\> Local

  safeLocation       Single line text

  deliveryStatus     Single line text
  ----------------------------------------

> Your Order_CRM table should look like and empty version of the one
> below:

![](./images/media/image11.png){width="6.9028696412948385in"
height="0.9587062554680665in"}

> Your Airtable Base is now ready for the Bootcamp!!! Let´s now setup
> the API that will allow you to access your data from your flows.

### Setting up your Airtable API

Airtable\'s API uses token-based authentication, allowing users to
authenticate API requests by inputting their tokens into the HTTP
authorization bearer token header. Two type of tokens are supported:
personal access tokens and OAuth access tokens. As this is for a demo
environment, we will use [Personal Access
Tokens](https://airtable.com/developers/web/guides/personal-access-tokens)
(PAT).

To create your PAT, go to <https://airtable.com/create/tokens> and click
on "Create token".

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image12.png){width="3.595240594925634in"
height="2.385931758530184in"}

- Now, give your token a name: "WxCCPartnerBootcamp".

- Define the Scope for your token. You will need to select
  **data.records:read** and **data.records:write**, as we will need R/W
  access to the data.

- Select your workspace to enable API access to your database.

- Click on "Create Token"

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image13.png){width="3.6569663167104114in"
height="3.0254418197725284in"}

You should see a confirmation panel with your newly created token. This
will be the only time you will see your token, it will never be shown
again, so make sure you copy it in a safe place for further use. Once
saved, you can click on "Done".

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image14.png){width="3.5972222222222223in"
height="2.2083333333333335in"}

Now you will see your new token in your Personal access token site:

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image15.png){width="5.778260061242345in"
height="1.7569969378827646in"}

Let's now generate the API documentation for your Airtable Base:

1.  **Go to https://airtable.com/api**\
    This is Airtable's official API documentation portal.

2.  **Log into your Airtable account** (if you're not already logged
    in).

3.  You'll see a list of **Bases** you have access to.\
    Click on your **Base**.

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image16.png){width="4.811069553805774in"
height="3.491246719160105in"}

4.  Airtable will generate **auto-documented API reference** for that
    Base, which includes:

- Authentication setup

- Endpoints for each **Table**

- Example **GET**, **POST**, **PATCH**, and **DELETE** requests

- Field names and data types

- Sample code snippets (e.g., using curl, JavaScript, etc.)

![A screenshot of a computer screen AI-generated content may be
incorrect.](./images/media/image17.png){width="5.32485564304462in"
height="3.64329615048119in"}

Bookmark this page as it will be your API reference for your base.

As you can see on the documentation, The **Airtable API URL** to access
a base's content is constructed using a specific format that includes
your **Base ID**, **Table Name (or ID)**, and the **API version**:

*https://api.airtable.com/v0/{{baseId}}/{{tableId}}*

- {{baseId}} -- A unique identifier for your base (e.g.,
  app1234567890abcdef).

- {{tableId}} -- The unique identity of the table you want to access.
  (e.g., tblABCDsGD34DS49J2). The tableId can be replaced by the
  {{tableName}}. If the table name contains spaces or special
  characters, it should be **URL-encoded** (e.g., My%20Table). To avoid
  formating issues, we recommend you to use {{tableld}}

Where you can find this information? This is detailed in the generated
API documentation for your base, but also is availble in your base
itself. If you go to your Base page, here is the data:

*{{tableName}} {{baseId}} {{tableId}} }}{{tableName}}{{tableName}}
{{tableName}} {{tableId}}*

Take note of those values.

### Build your Airtable API collection in Postman

NOTE: you can create records, modify information and visualize the data
in your Airtable Base through the Airtable web interface. So strictly
speaking, you would not need to define an Airtable API collection in
Postman for the Bootcamp labs. But this collection in Postman will allow
you to test and verify that your Airtable API is working fine, as we
will use the API from our flows to read and write data in the Order_CRM.

We will setup a basic API collection in Postman so that we can
manipulate your Base data through the API and test it is working
correctly. You must follow the below steps:

1.  Download this Postman collection for your Airtable API set:
    [Airtable - Bootcamp
    CRM.postman_collection.json](https://cisco.sharepoint.com/sites/WebexCCTechnicalBootcamp/_layouts/15/download.aspx?UniqueId=993109b8a1c140b3ba6395c9676700d0&e=a7UQPW)

2.  Log into Postman desktop app and click on "Import" or go to
    File-\>Import, navigate to the file you downloaded and import it
    into Postman. You should now see a full collection of
    "Airtable-Bootcamp CRM" API calls structured for Postman.

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image19.png){width="3.8627744969378828in"
height="2.0601465441819773in"}

3.  Now we will change some of the collection settings for this new
    collection to make it work for your Airtable Base.

    I.  Select the Airtable-Bootcamp CRM collection and then click on
        the Variables tab. Fill in the variables with your Airtable base
        identities (base-id and table-id) and the bearer-token with the
        PAT you generated for your Base. Leave the orderId blank for the
        moment. Click Save when updated.

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image20.png){width="5.095956911636046in"
height="2.285918635170604in"}

II. Click on the Authorization tab and verify the Auth Type is set to
    Bearer Token and the actual Token is set to the variable
    {{bearer-token}}.

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image21.png){width="4.2310804899387575in"
height="2.1504265091863517in"}

III. Filter your GET operation to selected records: Click on the "Get
     Order" request. Then go to the params tab. Disable the populated
     "maxRecords" and "view params". We will not use them. Create a new
     param called "filterByFormula" and assign it the value
     "orderId={{orderId}}". This will allow us to filter the GET
     operation for a specific order record. Click Save.

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image22.png){width="4.938780621172353in"
height="2.004511154855643in"}

> You can now test your API. Disable the "filterByFormula" param and
> click on "Send". This should return the records available in your
> base. You should get a 200OK response and the content of the available
> records. If that is not the case, check the value of your variables
> and token.
>
> If the Base is empty, you will get this JSON response:
>
> {
>
> \"records\": \[\]
>
> }
>
> If you have an empty record in the Base, this is what you get:
>
> {
>
> \"records\": \[
>
> {
>
> \"id\": \"reclOznQZ7SSNcV0r\",
>
> \"createdTime\": \"2025-04-09T10:06:36.000Z\",
>
> \"fields\": {}
>
> }
>
> \]
>
> }
>
> Note the "id" value in the response is the Record identity. This will
> be required if you want to modify any field in your record via a PATCH
> operation.

IV. Let´s now create a new record to test your API. Go to the POST --
    Add Order request. Under the Body tab, select "raw" and "JSON"
    format. Overwrite the content there with the following one:

> {
>
> \"fields\": {
>
> \"orderId\": \"41223335467\",
>
> \"consentComs\": \"True\",
>
> \"fName\": \"John\",
>
> \"lName\": \"Doe\",
>
> \"phoneNumber\": \"41223335467\",
>
> \"email\": \"jdoe@domain.com\",
>
> \"address\": \"2345 Nowhere str, Miami, USA\",
>
> \"timezone\": \"America/Miami\",
>
> \"productName\": \"Core Trio QI Charger\",
>
> \"deliveryAddress\": \"2345 Nowhere str, Miami, USA\",
>
> \"deliveryETA\": \"2025-04-30\",
>
> \"altDate1\": \"2025-05-05\",
>
> \"altDate2\": \"2025-05-06\",
>
> \"safeLocation\": \"Front Door\",
>
> \"deliveryStatus\": \"\"
>
> }
>
> }
>
> This should match the JSON format of your Base structure. If you have
> changed any of the proposed field names, make sure you also change the
> name in the JSON body above. Fields in the JSON structure must match
> the fields in your Airtable Base.
>
> TIP: you can always go to your Base **auto-documented API reference**
> and copy the JSON structure from there.
>
> Click on Send. You should get a 200 OK response with the newly created
> record content.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image23.png){width="5.38213801399825in"
> height="4.425347769028871in"}
>
> Now you can check on your Base the new record has been created.

![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image24.png){width="7.315265748031496in"
height="1.1085695538057743in"}

V.  To complete the verification of your Airtable API, let´s now modify
    a field in your new record. As you can see, the deliveryStatus field
    is empty. Let´s change it into "not Shipped". First you need to get
    the record identity of the record you want to modify. That is
    available in the response you got when you created the record. Take
    note of it.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image23.png){width="5.469396325459318in"
> height="2.023073053368329in"}
>
> Now in Postman, go to your PATCH-Modify Order Request. Hover on the
> {{record-id}} variable in the operation URL and insert the value of
> our record id.
>
> Make sure the Body of your request is in raw-JSON format and has the
> following content:
>
> {
>
> \"fields\": {
>
> \"deliveryStatus\": \"not shipped\"
>
> }
>
> }
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image25.png){width="6.059318678915136in"
> height="1.7667957130358705in"}
>
> Click on Send. You should get a 200 OK response with the modified
> record in the body. Check in your Airtable Base that the value has
> been updated.
>
> NOTE: if you get a 404 Not Found error, try changing the name of the
> variable {{record-id}} to, for instance {{recordid}} or directly
> remove the variable and paste the actual value instead.
