# Partner Lab Tenant Capabilities & Pre-requisites

## Overview

Welcome to the 2026 Webex Contact Center Partner Bootcamp — an immersive, hands-on experience designed to equip you with the skills to design, build, and demonstrate next-generation customer journeys powered by AI.
Over two days, you will implement a complete, real-world use case end-to-end in your own lab tenant, leveraging the latest Webex Contact Center capabilities. 

The goal is to show how intelligent automation, deployed as a proactive front door, can resolve customer needs before they become inbound volume — dramatically reducing agent workload while delivering a faster, more consistent customer experience.

By the end of the Bootcamp, you will have built and tested a live, end-to-end solution spanning outbound campaign management, autonomous AI Agent design, real-time workflow orchestration, human escalation with AI Assist, and a fully instrumented agent desktop — ready to demo, ready to adapt, and ready to take to your customers.

The bootcamp is structured as a progressive, hands-on implementation, where each lab builds on the previous one to create a complete customer journey.

You will work within your own WxCC tenant (Gold Tenant or NFR demo/lab) to deploy a production-like scenario that combines all the capabilities mentioned above.

This section describes the capabilities and configuration that must be in place in your **Gold Tenant (GT)** or **NFR Demo/Lab tenant** before attending the Bootcamp. It covers both the general platform setup required for all labs and the specific AI and Campaign Manager SKUs required for the advanced use cases.

???+ warning "Contact us if any capability is missing"
    If your tenant is missing **any** of the features or capabilities described in this section, please contact **jucorral@cisco.com** before the Bootcamp with the following information:

    | Required Information | Description |
    | :--- | :--- |
    | **Partner Name** | Your company / partner organisation name |
    | **Contact Person Email** | Your email address |
    | **Tenant Org ID** | Your Gold Tenant/NFR Demo lab Org_id, found in Control Hub → Account |
    | **Tenant Org Name** | The name of your WxCC tenant organisation |
    | **Tenant Data Center** | e.g. US1, EU2, ANZ1... You can easily find it in the URL of your Agent Desktop app |
    | **Missing Functionality** | Describe the specific feature(s) not available |

    Do not wait until the day of the Bootcamp — provisioning requests may take time to process.

---

## Gold Tenant Capabilities

The Bootcamp labs require the following **advanced SKUs** to be provisioned and active in your Gold Tenant. These are in addition to the standard Webex Contact Center configuration detailed in the sections below.

### AI SKUs — Webex AI Agent & AI Assistant

Labs 2 and 3 are built entirely around **Webex AI Agent** (Autonomous) and **Webex AI Assistant** features. Both must be enabled in your tenant.

To verify availability:

- **AI Agent Studio**: accessible from Control Hub → Contact Center → Quick Links → **Webex AI Agent**. If the link is absent or access is denied, the AI Agent SKU is not provisioned.
- **AI Assistant**: accessible from Control Hub → Contact Center → Desktop Experience → AI Assistant settings. If the configuration panel is not available, the AI Assistant SKU is not provisioned.

???+ tip "How AI SKUs are provisioned on Partner Lab Tenants"
    For existing Webex Contact Center Partners, the legacy Gold Tenant has been replaced by an **NFR Demo/Lab** tenant. The NFR Demo/Lab is provided at a 100% discount and includes the following items: 

    - 10 Premium Concurrent Agent Licenses (includes 2 IVR ports per agent)
    - 2 Webex AI Agent Units
    - 2 Webex AI Assistant Units

    All partners with a registered **Gold Tenant** must complete the transition to the NFR Demo/Lab. The detailed instructions to complete this transition are provided in the [NFR Demo/Lab General Information](https://cisco.sharepoint.com/sites/WxCCPartnerEnablement/SitePages/NFR-Demo-Lab-Main.aspx) site.

    **For the purposes of this Bootcamp, completion of the NFR transition is not mandatory. If AI capabilities are not enabled in your tenant, please contact us using the instructions above to request temporary access to the necessary features.** 

    *Following the Bootcamp, if you have not yet completed the NFR transition in your Gold tenant, please ensure it is finalized to secure permanent access to the AI features.* 

### MCP fulfillment

The autonomous AI agents now support Model Context Protocol (MCP) fulfillment actions. This allows AI agents to function as MCP clients that connect directly to remote MCP servers, thus eliminating the need for complex, custom-built API integrations.

???+ webex "Verify MCP availability in your tenant"

    ???+ inline end "MCP actions"

        <figure markdown>
        ![MCP actions](./assets/MCP%20actions.jpg)
        </figure>

    1. To verify that MCP actions is active in your tenant, log in to the **Webex AI Agent Studio** platform.

    2. Choose an **autonomous** AI agent in your dashboard. (If you don´t have any, you can create a new dummy one).

    3. Navigate to the Actions tab and click **[+Add actions]**.

    4. In the Add actions pop-up, under Browse actions you should see the option  **Select available** (see picture).

    **If you do not see this option in the **Actions** menu, please contact us using the instructions above.**


### Campaign Manager

Lab 1 requires the **Webex Campaign Management** add-on module to be enabled in your WxCC organisation. When this module is provisioned, you will have received an activation email similar to the one below:

???+ info "Campaign Manager activation email"
    ```
    Subject: Native Campaign Manager tenant ready for Org name = <OrgName>
    
    The Webex Campaign Management module has now been enabled for your WxCC org.

    You can access it at the following URL:
    https://<OrgName>.wxcc.webexcampaign.us/nextgen

    Org ID:   <your-org-id>
    Org Name: <your-org-name>
    ```

    If you have received this email, your Campaign Manager module is active. Navigate to the URL provided to confirm access. You should be able to access with your Control Hub admin credentials. 

For partners attending the Bootcamp that have not the Native Campaign Manager feature enabled, we have requested the provisioning of the feature in their Gold Tenant. We provided the email of one of the attendees to the Bootcamp. To verify availability, check whether you received the activation email above, and confirm you can log in to the Campaign Manager portal. If you cannot access Campaign Manager from the provided URL contact us following the instructions above. 

NOTE that there is no cross launch link from Control Hub yet for the Campaign Manager.

???+ tip "Additional Campaign Manager resources"
    The activation email contains also a few resources that provide a useful orientation to the Campaign Manager module:

    - **Official documentation**: [docs-campaign-for-contact-centers.webexcampaign.com](https://docs-campaign-for-contact-centers.webexcampaign.com/)
    - **Campaign Manager deep-dive playlist** (recommended for TSAs, SEs, and technical sellers): [Vidcast Playlist — Native Webex Campaign Management](https://app.vidcast.io/playlists/9dc8df9a-aad5-480e-bcf7-0cc57764e213) — 3 Vidcasts covering the full feature set of the add-on in detail.
   

### Email Digital Channel

Lab 2 requires a functional **Email digital channel** configured in Webex Connect to deliver messages to the customer. If you do not already have an email asset configured in your tenant, refer to the [Email Digital Channel setup guide](./pre_req_email_channel.md) included in this Bootcamp documentation.

---

## General Platform Pre-requisites

The following sections cover the baseline configuration that must be completed in your Gold Tenant / NFR Demo/lab for all attendees to be able to run the Bootcamp labs.

???+ warning "Naming convention recommendation"
    If attendees will be **sharing common resources** in the Gold Tenant, it is strongly recommended to define a naming convention for all per-attendee items (Teams, Entry Points, Flows, etc.) to avoid conflicts and confusion.

    Suggested convention: **prefix every item with the attendee's initials.**

    *Example for John Doe:* `JD_Bootcamp_Team`, `JD_Bootcamp_Voice_EP`, etc.

    Working as a team on shared resources is also acceptable if preferred.

### Administration Access

Every Bootcamp attendee must have access to the platform with the correct administrative roles.

???+ webex "Configure Administration Privileges"

    Each attendee's user account in Control Hub must be configured with the following:

    - **Contact Center licence**: assign the user an **Administrator** entitlement. This is set under the user's **Licences** section in the **Summary** tab of their Control Hub profile.
    - **Control Hub administrator role**: enable **Full Admin** under Administrator roles in the user profile.
    - **Webex Connect administrator role**: enable at least **Full Access** with **Decryption Access** for every attendee under the **Teammates** page in the Webex Connect portal.

        ???+ tip
            Decryption access is required for attendees to use the flow debugger and decrypt session logs in the AI Agent Studio — both of which are essential for troubleshooting during the labs.

### Basic Contact Center Setup

The following entities must be configured in Control Hub before the Bootcamp:

???+ webex "Configure Contact Center Entities"

    **1 — Bootcamp Site**

    A calling **Location** must exist in Control Hub for the Bootcamp. Without a location, phone numbers cannot be mapped to Entry Points. A single site shared by all attendees is sufficient.

    **2 — Bootcamp Team**

    To avoid conflicts with call routing to agents, a **Team per attendee** is recommended.

    - Assign the team to the *Bootcamp Site*.
    - Create it as **Agent Based**.


### Telephony Setup

???+ webex "Configure Telephony"

    A **DN (Directory Number)** assigned for outbound calls to the Contact Center will be required for the Bootcamp.

    - One DN per attendee must be available and mapped to their Voice Entry Point.
    - Attendees will use their **own mobile device** to simulate the customer and receive outbound calls from the Contact Center during the labs.

### Users Setup

Every attendee requires an agent user role configured in Control Hub with the correct licence and profile assignments.

???+ webex "Configure Agent Users"

    For each attendee's agent user account, apply the following settings under **Contact Center** → **Contact Centre Users**:

    - **User Profile**: assign a **Premium Agent** user profile.
    - **Contact Center slider**: enabled.
    - **Site**: assign the *Bootcamp Site*.
    - **Teams**: assign the *Bootcamp Team* (both agent and supervisor users must be assigned to the same team).
    - **Desktop profile**: assign an Agent profile.
    - **Multimedia profile**: assign `Default_Multimedia_Profile` (or equivalent).

    ???+ warning "Verify user activation status"
        Ensure all users are **activated** before the Bootcamp. Their status in Control Hub must have changed from *Not Verified* to **Active**. Unverified users cannot log in to the Agent Desktop.

### Basic Control Hub Settings

The following tenant-level settings must be verified and enabled:

???+ webex "Verify Control Hub Tenant Settings"

    **Digital Channels & Media Platform**

    - Confirm **Digital Channels** are enabled in your Gold Tenant.

        `Navigate to: Control Hub -> Contact Center -> Tenant Settings -> General and verify **Digital Channel* service is enabled with **Webex Connect**`

    - Confirm the **Real-Time media service** is configured as the **Voice media platform**. This is mandatory for **WebRTC** and **Cisco Cloud Text-to-Speech** to function correctly.

        `Navigate to: Control Hub → Contact Center → General → Voice media platform`

    **End Call Button**

    - Enable the **End Call** button in the Agent Desktop to allow agents to terminate calls from the desktop.

        `Navigate to: Control Hub → Contact Center → Tenant Settigs -> Desktop → Voice Features -> Enable End Call → Save`

## Auxiliary Applications

The Bootcamp use case relies on **Airtable** as its customer data backend — storing customer profiles, transaction history, and investment portfolio data that the AI Agent queries in real time during calls. Before starting the labs, you will need to set up your own Airtable base and populate it with the required table structure. Full setup instructions, field definitions, and sample data guidelines are provided in the [Airtable Database Setup](pre_req_airtable.md) section of this documentation.