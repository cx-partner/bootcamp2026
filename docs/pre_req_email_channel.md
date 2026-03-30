# Email

## Introduction

For the purposes of this Bootcamp, we are using Email to trigger proactive customer engagements. Although SMS or WhatsApp are more ubiquitous for 'near-instant' communication, the email channel was chosen to minimize configuration complexity. We will proceed with this setup, acknowledging that the response time may be less immediate than in a production-grade SMS environment.

Partners with SMS-enabled tenants may choose to utilize that channel at their discretion. In such cases, the email nodes within the flows should be replaced with their equivalent SMS counterparts to maintain the same logic.

Should you do not have an email channel enabled in your tenant, or prefer to setup a new one for the Bootcamp, this guide provide the necessary steps to configuring an Email Asset using Gmail as the provider. For a comprehensive overview of the configuration process, please refer to the [Product Guide](https://help.webexconnect.io/docs/wxcc-email-asset-creation). 

The implementation consists of the following steps: 

* [x] **Gmail Account Readiness**: Ensure a dedicated Gmail account is available for testing.
* [x] **Google Cloud Configuration**: Enable and configure an OAuth 2.0 Client ID within the Google Cloud Console to authorize API access.
* [x] **Webex Connect Integration**: Register the Email Asset within Webex Connect using the OAuth credentials and Gmail SMTP settings.
* [x] **Inbound Routing**: Configure a Forwarding Rule in Gmail to redirect incoming emails to the Webex Connect platform.
---

## Gmail Account Readiness  
Create a dedicated (or use and existing) gmail account. 

---
## Google Cloud Configuration
To integrate Gmail with Webex Connect, you must configure an OAuth 2.0 Client ID within the Google Cloud Console to establish a secure, token-based authorization link. This industry-standard protocol is required because Google has deprecated "Less Secure Apps" (basic password authentication) in favor of scoped access, which allows Webex Connect to send and receive emails without ever accessing your primary account credentials. 

???+ webex "Google Cloud Configuration"

	- Go to the [Google Console](https://console.cloud.google.com). Login with the gmail user credentials
	- Select an existing project or create a new one.

		???+ GIF inline end "Screenshot - OAuth consent configuration"
			<figure markdown>
			![Consent configuration](./assets/email%20OAuth%20consent.gif)
			</figure>

	- Go to **API & Services** 

		- Go to **OAuth consent screen**
			- Click on **Get started**
			- Provide an App name: `WxConnect`
			- Select the user support email: `you can use the same gmail`
			- Click **[Next]**
			- In the **Audience** select *External*
			- Click **[Next]**
			- Fill in the **Contact Information** and click [Next]
			- Agree with the *Google API Services: User Data Policy*
			- Click **[Continue]**
			- Click **[Create]**
		
		- From the **OAuth consent screen**, go to **Branding**
			- Scroll down to **Authorized domains** and click on **[+ Add domain]**
			- Add the <copy>webexconnect.io</copy> domain.
			- Click on **[Save]**
		
		- From the **OAuth consent screen**, go to **Audience**
			- make sure you add your gmail account as Test Users. If the app has not completed the Google verification process, it remains in *testing* status, and can only be accessed by developer-approved testers.
			- Under **Test Users** click on **[+ Add users]** and add your gmail account. 
		
		???+ gif inline end "OAuth client creation"
			<figure markdown>
			![Consent configuration](./assets/email%20OAuth%20client%20create.gif)
			</figure>


		- From the **API & Services** menu, go to **Credentials** 

			- Click on **[+Create credentials]**
			- Select **OAuth client ID**
			- Select *Web Application* in the **Aplication type** drop down menu
			- Set a name for the client: *WxConnect*
			- Under **Authorized JavaScript origins**, add your Webex Connect URI. It takes the following form: 
			
				<copy>https://YourTenantName.us.webexconnect.io</copy>. 
			
				You can copy it from the Webex Connect portal URL. Note that it does not include the last `/`.

			- Under **Authorized redirect URIs**, add 
			
				<copy>https://YourTenantName.us.webexconnect.io/callback</copy>
				
				This information is in the Configure New Application - Email page, in WxConnect portal, under the Call Back URL field.

			- click on **[Create]**

	The OAuth Client is now created for your email app and a Client ID and Client Secret have been generated. Copy those values as you will need them to configure your email application in Webex Connect.  


---
## Webex Connect Email Integration
Now that your Google Cloud credentials are ready, the third step is to register the Email Asset within the Webex Connect platform. This process serves as the formal "handshake" where you input your Client ID and Client Secret to authorize Webex Connect to act on behalf of your Gmail account. By configuring this asset, you enable the platform to listen for incoming messages and programmatically trigger outbound emails, effectively transforming a standard mailbox into a powerful communication node within your flows. 

???+ webex "Webex Connect Email Integration"

	???+ GIF inline end "Webex Connect Email asset configuration"
		<figure markdown>
		![Consent configuration](./assets/email%20asset%20create.png)
		</figure>
	
	- From the left navigation pannel in your Webex Connect portal, click **Assets** and then click **Apps**.
	- In the next page, click **[Configure New App]** and select **Email**
	- Give an name to the asset: `wxccpartnergmail`	
	- Populate the Email ID with your Gmail: `wxccpartner@gmail.com`
	- Note the prepopulated **Forwarding Address**. You will need this to set the forwarding rule in your gmail account.	
	- In the **Authentication Type** select *OAuth 2.0* 
	- Populate the mail server parameters as below: 

		| Parameter 		| Value   						|  
		| :---------- 		| :--------------				|  
		|**SMTP Server** 	|<copy>smtp.gmail.com</copy> 	|
		|**Username** 		|<the gmail username> 			|			
		|**Port** 	|<copy>465</copy> 	|				
		|**Security** 	| `SSL`	|				
		|**Client ID** 	|Copy the Client ID of your OAuth client	|				
		|**Client Secret** 	|Copy the Client Secret of your OAuth client	|				
		|**Authorization URL** 	|<copy>https://accounts.google.com/o/oauth2/auth</copy> 	|				
		|**Scope** 	|<copy>https://mail.google.com/</copy> 	|				
		|**Access Token URL** 	|<copy>https://oauth2.googleapis.com/token</copy> 	|				
		|**Refresh Token URL** 	|<copy>https://oauth2.googleapis.com/token</copy> 	|				

	???+ info
		If you did not do it in the previous step, to copy the Client ID and Client Secret you must go back to your Google console portal: go to **APIs & Services**, select **Clients** and then select your *OAuth client*. In the next page you will see the values under the **Additional Information** section on the right. If the existing Client Secret has been disabled for copying, you will need to generate a new one.

	- Click on **[Generate Token]** and follow the instructions in the Google screen
		- Select your gmail account
		- In the next window click on **[Continue]**
		- In the next window, click again on **[Continue]**
		- Your access token is now created. Click on **[Save]**
		- Make sure you now register the application by clicking on the top right button **[Register to Webex Engage]**
			- Select the Service and click **[Register]**

---
## Gmail Inbound Routing
The final step in this lab is to establish a Gmail Forwarding Rule, which ensures that any inbound email messages are instantly redirected to the Webex Connect platform. This process involves a security handshake: Google will issue a verification email containing a unique validation URL to your Webex Connect address. To "catch" this URL, you will deploy a simple Email Flow to intercept the incoming message and use the Flow Debugger to extract the link from the raw metadata. Completing this step creates the closed-loop system necessary for your AI Agent to handle real-time, two-way email conversations. 

???+ webex "Create the email flow in Webex Connect"
	
	Let´s first create our email flow in Webex Connect.
	
	???+ GIF inline end "Webex Connect Email flow"
		<figure markdown>
		![Create flow](./assets/email%20create%20flow.png)
		![Email flow](./assets/email%20flow%20nodes.png)
		![Make Live flow](./assets/email%20flow%20make%20live.png)
		</figure>

	

	!!! download "Email workflow"
		Download the [Email Echo flow.workflow](./bcamp_files/Email%20Echo%20Flow.workflow).

	- Go to your Webex Connect service.
	- Click on **Flows** and then on **[Create Flow]**
	
		- In the **Method** drop down menu, select **Upload a flow** 
		- Populate a name for your flow: <copy>Echo flow</copy> 
		- In the **Attachment** section, select the downloaded workflow
		- Click on **[Create]**

	- In the **Email Start** node, remove all the conditions if any, and click on **[Save]**

	- Now, let's make sure we leave the flow ready for debugging. 
		- Click the :fontawesome-solid-gear: button at the top right of the flow editor. 
		- Enable the **Descriptive logs** (set an appropriate time for the test).
		- Click **[Save]**

	- Save the flow with the top right **[Save]** button
	- Click the **[Make Live]** button to make the flow live. 
	- In the Make Live pop up window, select your email asset. Click again in **[Make Live]**.


???+ webex "Set the forwarding rule in Gmail"

	Let´s now configure the rule in Gmail to forward the emails to WxConnect.

	- Navigate back to the email channel app asset configuration screen in WxConnect and copy the value of the **Forwarding Address**.
	- go to your gmail account and on the **email Settings** select **Forwarding and POP/IMAP**

		- Click on **[Add a forwarding address]** and enter the *Forwarding address* from your email asset. 
		- Click **[Next]**
		- Enter your credentials in the next login page
		- In the next dialgue, click **[Proceed]**
		- You will see the following message: 

			`A confirmation link has been sent to 4475e4a8dxxxxxx8a108d26d8beeea04@mail-us.imiconnect.io to verify permission.`

		- click **[OK]**

???+ webex "Extract the verification URL"

	???+ inline end "Screenshot - flow debugging"
		<figure markdown>
		![Flow Debugging](./assets/email%20echo%20verification%20URL.png)
		</figure>


	- Go back to your *Email Echo* flow in WxConnect. 
	- click on the *s*mall bug icon* in the right panel of the flow editor
	- A **Transaction Logs** panel will pop up at the bottom of the canvas 
	- You should have a transaction that corresponds to the incoming verification email. Refresh the information if You do not see it. It may take few secods to appear. 
	- Click on the transaction
	- Click on the **DECRYPT LOGS** option in the top right of the transactions panel
	- Click on the first node in the transaction log and in the  *Details** panel on the right, locate the verification URL in the message body. It follows the text: *please click the link below to confirm the request:*. 
	- Copy the complete URL and paste in a browser. It will take you to the confirmation page. 
	- Click on **[Confirm]**. 

	Your email asset is now fully functional in your tenant!


{==Congratulations!!! You are now done with your email channel. ==}


???+ Challenge "Testing your email asset"
	
	You can test your email asset by sending an email to your email asset address. The echo flow will send a confirmation response back. Your flow will respond with something like: 

		Hi <name>.
		we have got your email with Subject : <subject_of_the_incoming_email> 
		and body: 

		<body_of_the_incoming_email>
	


	
 


