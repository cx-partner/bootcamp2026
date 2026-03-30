# Lab 3 - Human Escalation and RT Assist 

## Lab Purpose

In **Lab 2**, you successfully configured a **Webex AI Agent** to automate the collection of debts and handle customer inquiries regarding their accounts. 

In this lab we will focus on the transition from the Webex AI Agent to a fraud specialist, ensuring that the full context of the interaction is preserved during the transfer. We will explore key **Webex Contact Center AI Assistant** features, including **Summarization**, **Real-Time Assist (RTA)**, and **Real-Time Transcription (RTT)**. This guide covers the configuration required to extract data from the AI Agent metadata at the time of escalation, enable summarization and RTT, and configure an AI Assistant skill to support the agent during a fraud-related scenario.

???+ purpose "Lab Objectives"
    The purpose of this lab is to escalate a call from an AI Agent to a fraud specialist while maintaining full context and providing the agent with the tools necessary to deliver a consistent, seamless customer experience. 
    
    Key objectives include:

    *   **Context Management:** Configuring the flow to extract context from the AI Agent metadata and pass it to the fraud specialist, including a Virtual Agent (VA) transfer summary.
    *   **Enable RTT and Summarization Features:** Enhancing the fraud specialist workflow by enabling advanced AI-driven features.
    *   **Handle a Credit Card Fraud Scenario:** Providing real-time guidance based on the live conversation. This includes automating backend actions such as opening a case and locking a card, as well as retrieving relevant information from a Knowledge Base (KB).

???+ Challenge "Lab Outcome"
    By the end of this lab, you will have successfully implemented a sophisticated escalation workflow. Specifically, you will be able to:

    *   **Verify Contextual Handoff:** Confirm that the fraud specialist receives the full interaction history, including the VA transfer summary and relevant customer metadata, immediately upon call arrival.
    *   **Validate AI Assistant Functionality:** Observe the Real-Time Transcription (RTT) and Summarization features in action within the Agent Desktop, ensuring they accurately capture and distill the conversation.
    *   **Demonstrate Real-Time Guidance:** Successfully trigger and view automated guidance prompts when the conversation shifts to a "fraud" scenario, assisting the agent in taking the correct next steps.
    *   **Automate In-Call Actions:** Ensure that the system triggers backend workflows—such as case creation and card locking—in real-time during the conversation, reducing manual effort and improving response speed for the agent.
---

## Pre-requisites

In order to be able to complete this lab, you must: 

* [x] Have your **Airtable repositories** completed for the *Customer* and *Transactions* data
* [x] Have an **email digital channel** available in your tenant
* [x] Have completed [Lab 2 - Automating Debt Collection](lab2_debt_ai_agent.md)

--- 

## Lab Overview 📌 

In this lab you will perform the following tasks:

1. Configure Alex to transfer to a fraud specialist with context. 
2. Setup the call flow to extract context from the AI Agent metadata, escalate to a queue and configure Real-Time Transcription. 
3. Setup AI Assistant features in Control Hub. 
4. Confirm your Desktop Layout is setup correctly for AI Assistant. 
5. Setup an AI Assistant skill for Real-Time Assist. 
6. Test the complete scenario 

---

## Lab 3.1 Configure Alex to transfer to a fraud specialist with context. 

Our AI Agent for debt collection (Alex) can perform multiple tasks (check balance, authentication, check recent transaction, etc), when transferring to a human agent, its essential to make sure the data collected by Alex is not lost during the transfer. We will setup a custom transfer action and pass data in the form of entities into the flow. 

???+ webex "Alex Setup"
    1. From [Control Hub](https://admin.webex.com) navigate to **[Contact Center]** and under **Quick Links** click on the **Webex AI Agent** link. The Webex AI Agent studio will open in a new window. 
    2. In the **AI Agent Studio**, select your *Finance Debt Collection Agent*
    3. Click on **Actions**
    4. Click on **Create new** and select **Transfer**
    5. Fill in the **General information** of your action
        - **Action name**: <copy>fraud_transfer</copy>
        - **Action description**: <copy>Action to transfer the call to a human agent when fraud is suspected</copy>
        - **Transfer visibility**: You can either enable or disable the **Announce transfer** toggle
    6. Select the **Add new entity** option, fill out the entity details with this information: 
        
        | Entity Name | Type | Description | Example | Required |
        | :----- | :--- | :--- | :--- | :--- |
        | <copy>`ivr_verified`</copy>  | `String` | <copy>`Send a true or false value depending if the user was successfully authenticated before executing this action.`</copy> | <copy>`True, False`</copy> |`Yes` | 
        | <copy>`debt_balance`</copy>  | `String` | <copy>`User's debt balance`</copy> | <copy>`10000`</copy> |`No` | 
        | <copy>`susp_trans`</copy>  | `Number` | <copy>`Amount of the transaction identified as suspicious`</copy> | <copy>`10000`</copy> |`No` | 
        | <copy>`susp_vendor`</copy>  | `String` | <copy>`Vendor of the transaction identified as suspicious`</copy> |  |`No` |
        | <copy>`susp_date`</copy>  | `Date` | <copy>`Date of the transaction identified as suspicious`</copy> |  |`No` |
    7. Click **Add**.
    8. Go back to the **Profile** tab and add the following into the **Instructions**: 
    ```
    **Escalation Logic:**
    If fraud is suspected, escalate to a Fraud Specialist by using the **[fraud_transfer]** transfer action.
    ```
    9. Click **Save Changes** and **Publish**. 
    ???+ gif "Transfer Action Setup"
        <figure markdown>
        ![Alt Text](./assets/transfer_action.gif)
        <figcaption>Transfer Action Configuration</figcaption>
        </figure>


## Lab 3.2 Setup Call Flow - AI Agent context, Queue escalation and RTT configuration 

In Lab 2, the *Escalated* outcome of the **Virtual Agent V2** node was connected to a temporary **Play Message** node. In this section, you will replace that temporary path with a production-ready escalation flow. This involves extracting the contextual data that Alex collected during the conversation, passing it to the human agent via the queue, and enabling Real-Time Transcription so the fraud specialist has a live view of the conversation from the moment it arrives

???+ webex "Setup Call Flow"
    When Alex triggers the `fraud_transfer` action, the entities defined in that action (e.g., `ivr_verified`, `debt_balance`, `susp_trans`) are passed into the flow within the metadata output variable from **Virtual Agent V2** node. You need to capture these into flow variables so they can be presented to the agent. 
    
    1. Go to **Control Hub** -> **Contact Center** -> **Flows**
    2. Open your flow **AI_Agent_DebtCollection**
    3. First, we need to create the flow variables that will hold the context from Alex. Click on the **Global Flow Properties** panel (the gear icon in the top-right of the flow editor).
    4. Under **Custom Flow Variables**, create the following variables:

    | Variable Name | Type | Default Value |
    | :--- | :--- | :--- |
    | <copy>`transfer_type`</copy> | `String` | <copy>`agent_transfer`</copy> |
    | <copy>`ivr_verified`</copy> | `String` | <copy>`False`</copy> |
    | <copy>`debt_balance`</copy> | `String` | <copy>`0`</copy> |
    | <copy>`susp_trans`</copy> | `String` | <copy>`0`</copy> |
    | <copy>`susp_vendor`</copy> | `String` | <copy>`Unknown`</copy> |
    | <copy>`susp_date`</copy> | `String` | <copy>`N/A`</copy> |

    5. Click **[Save]**

    ???+ gif "Create Flow Variables"
        <figure markdown>
        ![Set Escalation Context](./assets/lab3_set_escalation_context.gif)
        <figcaption>Mapping AI Agent output to flow variables</figcaption>
        </figure>

???+ webex "Map the Virtual Agent Output to Flow Variables"

    Now, you will extract the output from the **Virtual Agent V2** node and map it into the flow variables you just created.

    1. In your flow, click on the **Virtual Agent V2** node (named `DebtCollectionAgent`).
    2. In the **Activity Settings** panel on the right, scroll down to the **Output Variables** section. You will see the variables that the AI Agent passes back upon escalation, the MetaData variable contains the transfer action details. 
    3. Delete the temporary **Play Message** node connected to the *Escalated* outlet.
    4. Drag and drop a **Parse** node onto the canvas and connect it to the *Escalated* path of the VAV2 node.
    5. Click the **Parse** node and rename it <copy>`Parse_Transfer`</copy>. 
    6. In the Description of the node, add the following: <copy>'Collects the transfer type.'</copy>
    6. Configure the Parse Settings: 
        * Input Variable = DebtCollection_Agent.MetaData
        * Content Type = JSON
        * Output Variable = transfer_type
        * Path Expression = <copy>$.escalation_trigger</copy>
    7. Drag and drop a **Case** node in the canvas and rename it <copy>Transfer_Check</copy>
    8. In the Description of the node, add the following: <copy>'Checks the transfer_type variable to route accordingly'</copy>
    9. Configure the **Case** node settings:
        * Variable = transfer_type
        * Case 1 = <copy>'agent_transfer'</copy>
        * Case 2 = <copy>'fraud_transfer'</copy>
    10. Add a **Queue Contact** node for the standard agent_transfer and rename it <copy>Generic_Queue</copy>. 
    11. Connect the **agent_transfer** path from the **Case** node to the **Generic_Queue** node. 
    12. Drag and drop a **Parse** node onto the canvas.
    13. Connect the *fraud_transfer* path of the **Case** node to the new **Parse** node.
    14. Click the **Parse** node and rename it to <copy>`Fraud_Context`</copy>
    15. Map your custom variables to a JSON path location from the Virtual Agent MetaData:
        * Input Variable = DebtCollection_Agent.MetaData
        * Content Type = JSON
        * Output Variable = transfer_type
        * Path Expression = <copy>$.escalation_trigger</copy>


## Testing :test_tube:

[Explain how the user can verify their work is correct.]

???+ webex "Validation Steps"
    1. **Trigger the event**: [Describe how to trigger it, e.g., "Place a call to..."]
    2. **Observe the result**: [Describe what should happen, e.g., "You should see a pop-up..."]
    3. **Check logs**: Navigate to the **Logs** tab and verify the status is `200 OK`.

---

## Lab Completion ✅

At this point, you have successfully:

- [x] [Goal 1]

- [x] [Goal 2]

- [x] [Goal 3]

- [x] ...

**Congratulations!** You have successfully completed Lab X.X. You are now ready to move on to the next section.


[Next Lab: Lab 4 - Cross-skill multi-agent orchestration](./lab4_ai_agent_transfer.md){ .md-button .md-button--primary }