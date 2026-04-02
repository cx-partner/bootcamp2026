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

???+ webex "Prepare Call Flow"
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

???+ webex "Extract AI Agent Context"
    Now, you will extract the output from the **Virtual Agent V2** node and map it into the flow variables you just created.

    1. In your flow, click on the **Virtual Agent V2** node (named `DebtCollectionAgent`).

        ???+ inline end "Before - Flow View"
            <figure markdown>
            ![Before - Flow View](./assets/lab3_before_flow.png)
            </figure>
    2. In the **Activity Settings** panel on the right, scroll down to the **Output Variables** section. You will see the variables that the AI Agent passes back upon escalation, the MetaData variable contains the transfer action details. 
    3. Delete the temporary **Play Message** node connected to the *Escalated* outlet.
    4. Drag and drop a **Parse** node onto the canvas and connect it to the *Escalated* path of the VAV2 node.
    5. Click the **Parse** node and rename it <copy>`Parse_Transfer`</copy>. 
    6. In the Description of the node, add the following: <copy>`Collects the transfer type.`</copy>
    6. Configure the Parse Settings: 
        * Input Variable = DebtCollection_Agent.MetaData
        * Content Type = JSON
        * Output Variable = transfer_type
        * Path Expression = <copy>`$.escalation_trigger`</copy>
        !!! info "AI Agent MetaData"
            The AI Agent MetaData variable contains the executed actions and collected variables during a session. When the AI Agent escalates the call, the **escalation_trigger** variable in the metadata will match the name of the
            transfer action used. In the previous step, we are collecting this value to decide how the call escalation needs to be routed. 
    7. Drag and drop a **Case** node in the canvas and rename it <copy>`Transfer_Check`</copy>
    8. In the Description of the node, add the following: <copy>`Checks the transfer_type variable to route accordingly`</copy>
    9. Configure the **Case** node settings:
        * Variable = transfer_type
        * Case 1 = <copy>`agent_transfer`</copy>
        * Case 2 = <copy>`fraud_transfer`</copy>
        !!! info "Queue Setup"
            In the next steps you wil be adding Queue Contact nodes to your flow, the queue you select is not important, as we simply need the call to be routed to an agent account that is available to receive your call. In addition,
            make sure the agent is configured to use the Desktop Layout available here. 
    10. Add a **Queue Contact** node for the standard agent_transfer and rename it <copy>`Generic_Queue`</copy>. 
    11. Connect the **agent_transfer** path from the **Case** node to the **Generic_Queue** node. 
    12. Connect the **Generic_Queue** node output to the **End Flow** node. 
    12. Drag and drop a **Parse** node onto the canvas.
    13. Connect the *fraud_transfer* path of the **Case** node to the new **Parse** node.
    14. Click the **Parse** node and rename it to <copy>`Fraud_Context`</copy> 
    15. Map your custom variables to a JSON path location from the Virtual Agent MetaData, you need to add a new variable and path expression for each one:
        * Input Variable = DebtCollection_Agent.MetaData
        * Content Type = JSON
        * Output Variable = ivr_verified
        * Path Expression = <copy>`$.actions.fraud_transfer[0].input.ivr_verified`</copy>
        * Output Variable = debt_balance
        * Path Expression = <copy>`$.actions.fraud_transfer[0].input.debt_balance`</copy>
        * Output Variable = susp_transaction
        * Path Expression = <copy>`$.actions.fraud_transfer[0].input.susp_transaction`</copy>
        * Output Variable = susp_vendor
        * Path Expression = <copy>`$.actions.fraud_transfer[0].input.susp_vendor`</copy>
        * Output Variable = susp_date
        * Path Expression = <copy>`$.actions.fraud_transfer[0].input.susp_date`</copy>
        ???+ inline end "After - Flow View"
            <figure markdown>
            ![After - Flow View](./assets/lab3_after_flow.png)
            </figure>

    16. Add a **Queue Contact** node for the custom **fraud_transfer** and rename it <copy>`Fraud_Queue`</copy>. 
    17. Connect the **Parse** node output to the **Fraud_Queue** node. Connect the **Fraud_Queue** node output to the **End Flow** node.
    18. Click **Validate**, then **Save and Publish** the flow.
    ???+ gif "Extract AI Agent Context"
        <figure markdown>
        ![Set Escalation Context](./assets/lab3_set_escalation_context.gif)
        <figcaption>Mapping AI Agent output to flow variables</figcaption>
        </figure>




???+ webex "Configure Real-Time Transcription"
    Real-Time Transcription(RTT) provides a live text feed of the conversation to the agent. This is essential for the fraud specialist, as it allows them to review what was said even if the audio quality is poor or the customer speaks quickly.
    In order to configure RTT, we will need to both enable the AI Assistant configuration in Control Hub and setup the flow to stream the media to the transcription service. In this section, we will focus on the flow configuration. 

    1. In your flow, move from the **Main Flow** tab to the **Event Flows** tab. 
    2. Drag and drop the **Start Media Stream** node to the canvas. 
    3. Connect the **AgentAnswered** event node to the **Start Media Stream** node. 
    4. Drag and drop an **End Flow** node to the canvas and connect it to the output path of the **Start Media Stream** node.
    5. Click **Validate**, then **Save and Publish** the flow.
    !!! info "RTT Conditional Enablement"
        In this lab we are enabling RTT for all the queue or agents involved in this flow, but you can also add a condition to control which queues within a flow get access to the RTT feature. You can find a step by step guide [HERE](https://help.webex.com/en-us/article/n5jhgdi/Enabling-media-streaming-for-specific-queues).
    ???+ gif "Extract AI Agent Context"
        <figure markdown>
        ![Set Escalation Context](./assets/lab3_set_escalation_context.gif)
        <figcaption>Mapping AI Agent output to flow variables</figcaption>
        </figure>

## Lab 3.3 Control Hub Configuration of Contact Center AI Assistant and Desktop Layout Requirements
 There are several features associated with the Contact Center AI Assistant (AutoCSAT, Summarization, Agent Wellness, Real-Time Transcription, Real-Time Assist and Topic Analytics), and all of these features can easily be enabled from Control Hub. 
 In this lab, we will focus on the  Summarization, Real-Time Transcript and  Real-Time Assist configuration. 

???+ webex "AI Assistant Configuration"
    1. Open **Control Hub** and navigate to Contact Center > AI Assistant. 
    2. The first 2 options in this page will be for **Agent Wellbeing** and **AutoCSAT**, but this are not relevant for this lab. 
    3. Enable the **Generated Summaries**  toggle and select the check box for all of the different summarization types. 
    4. Summarization is enabled at the queue level, for the purpose of this lab, you can either select the **All queues** option or specify the **Individual queue** you will be using in this lab. 
    5. Enable the Real-Time Transcription and the Real-Time Assist toggles. 
    6. Before we can **Assign AI Assistant skills** to a queue, we need to create the skill in the AI Agent studio. We will cover this topic in lab 3.4. 
    ???+ gif "Extract AI Agent Context"
        <figure markdown>
        ![Set Escalation Context](./assets/lab3_set_escalation_context.gif)
        <figcaption>Mapping AI Agent output to flow variables</figcaption>
        </figure>

???+ webex "AI Assistant Configuration"
    The AI Assistant is part of the default Desktop layout, however we understand that you might have your own layout that has been previously customized to include other widgets. 
    We will review the steps to make the necessary changes to a Desktop layout for the AI Assistant and the Real-Time Transcription window to show up on the Agent Desktop. 
    !!! download "Knowledge base document"
        If you don't need to update your own Desktop Layout, download a template [HERE](./bcamp_files/Webex_Financial_Group_KB.docx) and ignore the next steps. 
    
    1. Open your JSON layout file using a text editor (Sublime, Notepad++, etc) or IDE (VS Code, etc). 
    2. Find the area section for each relevant persona (agent, supervisor, agent-supervisor). Inside the area section, locate the advancedHeader array.
    3. Add the AI Assistant component(**"comp": "ai-assistant"**) to the **advancedHeader** section, here's an example: 

        ```
        "advancedHeader": [
            {
              "comp": "digital-outbound",
              "script": "https://wc.imiengage.io/AIC/engage_aic.js",
              "attributes": {
                "darkmode": "$STORE.app.darkMode",
                "accessToken": "$STORE.auth.accessToken",
                "orgId": "$STORE.agent.orgId",
                "dataCenter": "$STORE.app.datacenter",
                "emailCount": "$STORE.agent.channels.emailCount",
                "socialCount": "$STORE.agent.channels.socialCount"
              }
            },
            {
              "comp": "agentx-webex"
            },
            {
              "comp": "ai-assistant"
            },
            {
              "comp": "agentx-outdial"
            },
            {
              "comp": "agentx-notification"
            },
            {
              "comp": "agentx-state-selector"
            }
          ],
        ```
    
    4. Now let's add the RTT window. Find the area section for each relevant persona (agent, supervisor, agent-supervisor). Inside the area section, locate the panel section.
    5. Within the panel section, ensure that the following code snippet (or a similar configuration for real-time transcription) is present:
    
        ```
        {
                "comp": "md-tab",
                "attributes": {
                  "slot": "tab",
                  "class": "widget-pane-tab"
                },
                "children": [
                  {
                    "comp": "slot",
                    "attributes": {
                      "name": "RT_TRANSCRIPT_TAB"
                    }
                  }
                ],
                "visibility": "RT_TRANSCRIPT"
              },
              {
                "comp": "md-tab-panel",
                "attributes": {
                  "slot": "panel",
                  "class": "widget-pane"
                },
                "children": [
                  {
                    "comp": "slot",
                    "attributes": {
                      "name": "RT_TRANSCRIPT"
                    }
                  }
                ],
                "visibility": "RT_TRANSCRIPT"
              },
        ```
    6. Assign the Desktop Layout to the Team of the agent you will be using to receive the call. 
    ???+ gif "Extract AI Agent Context"
        <figure markdown>
        ![Set Escalation Context](./assets/lab3_set_escalation_context.gif)
        <figcaption>Mapping AI Agent output to flow variables</figcaption>
        </figure>


## Lab 3.4 Setup an AI Assistant skill for Real-Time Assist.
Now that the AI Assistant features are enabled we can proceed with the configuration of Real-Time Assist (RTA). This feature monitors the live transcription of the call and, when specific keywords or intents are detected, it presents the agent with suggestions from a KB or triggers automated actions. 
During this lab section we will focus on how to effectively setup an AI Assistant Skill, including how to define the prompt and how to setup actions.

???+ webex "Configure Real-Time Assist"



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