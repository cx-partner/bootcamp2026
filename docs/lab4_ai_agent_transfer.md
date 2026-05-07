# Lab 4 - Cross-Skill and Multi-Agent Orchestration

## Lab Purpose

In **Lab 2**, you built **Alex**, an autonomous AI Agent specialized in debt collection. In **Lab 3**, you configured the escalation path from Alex to a human fraud specialist. In this lab, you will design a **multi-agent architecture** where Alex hands off to a second AI Agent — an **Investment Advisor** — that leverages an external **MCP (Model Context Protocol) Server** to provide real-time stock market data, portfolio management, and order execution.

This lab introduces the concept of **agent-to-agent transfer**, where one AI Agent recognizes that a customer's intent falls outside its domain and seamlessly transfers the conversation — with full context — to a specialized peer agent. You will also learn how to register and configure an **Agentic App** in the Webex Developer Portal, provision it in Control Hub, and connect it to your AI Agent's actions via MCP.

???+ purpose "Lab Objectives"
    The purpose of this lab is to extend your contact center automation beyond a single AI Agent by implementing a multi-agent orchestration pattern.

    Key objectives include:

    *   **Agent-to-Agent Transfer:** Configuring Alex to recognize investment-related intent and transfer the conversation to a specialized Investment Advisor AI Agent.
    *   **MCP Server Integration:** Connecting the Investment Advisor to an external MCP Server that provides stock market data, portfolio retrieval, and order execution tools.
    *   **Agentic App Registration:** Registering the MCP Server as an Agentic App in the Webex Developer Portal and provisioning it in Control Hub.
    *   **End-to-End Testing:** Validating the complete flow from debt collection through agent transfer to investment advisory with live MCP tool execution.

???+ Challenge "Lab Outcome"
    By the end of this lab, you will have:

    *   **A second AI Agent** (Investment Advisor) capable of providing stock recommendations, retrieving customer portfolios, and placing orders.
    *   **A seamless transfer** from Alex to the Investment Advisor with conversation context preserved.
    *   **Live MCP integration** where the Investment Advisor calls external tools hosted on a remote server to fetch real-time data and execute financial transactions.
    *   **A working payment flow** where the customer receives a NovaPay payment link via email to complete a stock purchase.

---

## Pre-requisites

In order to be able to complete this lab, you must:

* [x] Have your **Airtable repositories** completed for the *Customer*, *Investment*, and *Positions* tables
* [x] Have completed [Lab 2 - Automating Debt Collection](lab2_debt_ai_agent.md)
* [x] Have completed [Lab 3 - Human Escalation and RT Assist](lab3_human_ai_assist.md)

---

## Lab Overview 📌

In this lab you will perform the following tasks:

1. Configure a transfer action in Alex for investment-related requests.
2. Create the Investment Advisor AI Agent.
3. Register the MCP Server as an Agentic App in the Webex Developer Portal.
4. Provision the Agentic App in Control Hub.
5. Connect the MCP tools to the Investment Advisor.
6. Update the call flow to handle the agent-to-agent transfer.
7. Test the complete multi-agent scenario.

---

## Lab 4.1 Configure Alex to Transfer to the Investment Advisor

Just as we configured a `fraud_transfer` action in Lab 3, we will now create an `investment_transfer` action that Alex will use when a customer expresses interest in their investment portfolio, stock purchases, or market inquiries.

???+ webex "Create the Transfer Action in Alex"
    1. From [Control Hub](https://admin.webex.com) navigate to **Contact Center** and under **Quick Links** click on the **Webex AI Agent** link. The Webex AI Agent studio will open in a new window.
    2. In the **AI Agent Studio**, select your *Finance Debt Collection Agent* (Alex)
    3. Click on **Actions**
    4. Click on **Add Actions** and select **Transfer**
    5. Fill in the **General information** of your action:
        - **Action name**: <copy>investment_transfer</copy>
        - **Action description**: <copy>Action to transfer the call to the Investment Advisor AI Agent when the customer wants to discuss their investment portfolio, buy or sell stocks, or inquire about market data.</copy>
    6. Select the **+New Input entity** option, fill out the entity details with this information:

        | Entity Name | Type | Description | Example | Required |
        | :----- | :--- | :--- | :--- | :--- |
        | <copy>`customer_id`</copy> | `String` | <copy>`The customer ID retrieved during authentication`</copy> | <copy>`CUST-001`</copy> | `Yes` |
        | <copy>`customer_email`</copy> | `String` | <copy>`The customer email address`</copy> | <copy>`jdoe@domain.com`</copy> | `Yes` |
        | <copy>`investment_id`</copy> | `String` | <copy>`The customer investment account ID if available`</copy> | <copy>`INV-001`</copy> | `No` |

    7. Click **Add**.
    8. Go back to the **Profile** tab and add the following into the **Instructions**:
        ```text
        **Investment Transfer Logic:**
        If the customer wants to discuss investments, buy or sell stocks, check their portfolio, or inquire about market data, authenticate them and transfer to the Investment Advisor using the **[investment_transfer]** transfer action.
        ```
    9. Click **Save Changes** and **Publish**.

    ???+ gif "Investment Transfer Action Setup"
        <figure markdown>
        ![Investment Transfer Action](./assets/lab4_investment_transfer_action.gif)
        <figcaption>Configuring the investment transfer action in Alex</figcaption>
        </figure>

---

## Lab 4.2 Create the Investment Advisor AI Agent

In this section, you will create a second autonomous AI Agent — the **Investment Advisor**. This agent specializes in investment portfolio management, stock market data, and order execution. Unlike Alex, whose actions are fulfilled by Webex Connect flows, the Investment Advisor's actions will be fulfilled by an external **MCP Server**.

???+ webex "Create the Investment Advisor"

    1. From [Control Hub](https://admin.webex.com) navigate to **[Contact Center]** and under **Quick Links** click on the **Webex AI Agent** link.
    2. Click on **[+ Create agent]**
    3. Click on **[Start from scratch]** and then **[-> Next]**
    4. Select the **[Autonomous]** option and fill in the **essential details**:

        - **Agent Name**: <copy>`Investment Advisor Agent`</copy>
        - **AI engine**: Select one of the available engines
        - **Agent's goal**: Copy the below content

            <copy>`You are an Investment Advisor for Webex Financial Group. Your mission is to help customers manage their investment portfolio, provide stock market information, and execute buy or sell orders. You use real-time market data and the customer's portfolio to provide informed recommendations. Maintain a professional, knowledgeable, and helpful tone.`</copy>

    5. Click the **[Create]** button.
    2. From the AI Agent configuration page, fill in the **Welcome message**:
        ```text
        Hello! I'm your Investment Advisor from Webex Financial Group. I can help you check your portfolio, look up stock prices, and place buy or sell orders. How can I assist you today?
        ```

    3. Copy the below prompt in the **Instructions** field:
        ```text
        1. Identity
        Role Definition: You are an Investment Advisor AI Agent for Webex Financial Group, specializing in portfolio management and stock trading.
        Tone: Professional, knowledgeable, and helpful. Provide clear explanations of market data and investment options.

        2. Context
        Background: You handle investment-related conversations transferred from the Debt Collection Agent (Alex). The customer has already been authenticated.
        Environment: Voice line with potential background noise. Keep responses clear and concise.

        3. Steps:

        **Always collect the Investment ID before perform any other tasks**. You can use the tool [get_investment_account] to get it using the Customer ID (for example, CUST-002)

        Portfolio Review: If requested, use [get_portfolio] to retrieve and present current positions.
        Stock Inquiry: If the customer asks about a stock, use [get_stock_price] to provide current market data.
        Order Placement: If the customer wants to buy or sell:
            - Confirm the ticker, quantity, and action (BUY or SELL) with the customer.
            - Execute [initiate_order] to create a payment session and send the payment link.
            - Inform the customer that a payment link has been sent to their email.
            - Once the customer confirms they have completed the payment, execute [confirm_order] to verify and finalize the order.
        Summary: After each action, provide a clear summary of what was done.

        4. Available Stocks
        The following tickers are available for trading:
        - MOON (MoonShotMeme) - Tech sector
        - YLO (YOLO-Tech) - Tech sector
        - DEBT (InfiniteDebtInc) - Finance sector
        - CATV (CatVideoStreaming) - Entertainment sector
        - HYPE (HypeTrainHoldings) - Retail sector

        5. Response Guidelines
        Formatting: Keep responses short and conversational. When presenting portfolio data, summarize clearly.
        Language Style: Use clear, direct language. Explain market terms if the customer seems unfamiliar.

        6. Error Handling
        Clarification: "I didn't catch that. Could you repeat?"
        Default Response: "I can help with portfolio reviews, stock prices, and placing orders. What would you like to do?"
        Action Failures: "I'm experiencing a delay retrieving that information. Please hold while I try again."

        7. User Defined Guardrails
        Stay within Webex Financial Group's investment services.
        Never provide financial advice or guarantee returns.
        Always confirm order details with the customer before executing.
        The Airtable Base ID and API Key will be provided as configuration. Use them when calling portfolio and order tools.
        ```

    4. Click on **[Save changes]**

    ???+ tip "Investment Advisor vs. Alex"
        Notice the key difference: Alex is focused on **debt recovery** with strict security protocols and authentication flows. The Investment Advisor assumes the customer has **already been authenticated** by Alex and focuses purely on investment operations. This separation of concerns is a core principle of multi-agent architecture.

    ???+ gif "Create Investment Advisor"
        <figure markdown>
        ![Create Investment Advisor](./assets/lab4_create_investment_advisor.gif)
        </figure>

---

## Lab 4.3 Register the MCP Server as an Agentic App

???+ Important
    For sections 4.3 and 4.4, you must use a **Customer Administrator** account rather than a Partner account. Partner accounts do not have the permissions required to access the **Agentic Apps** configuration in Control Hub, which is the only way to authorize and enable specific MCP tools for your AI Agent.

The Investment Advisor's tools are hosted on an external MCP Server. To connect the AI Agent to this server, you must first register it as an **Agentic App** in the Webex Developer Portal.

???+ info "What is an Agentic App?"
    An Agentic App is a registered external service that exposes capabilities (tools, resources, or prompts) via the MCP protocol. By registering the MCP Server as an Agentic App, you allow Webex AI Agents to discover and call the tools it exposes, with authentication and governance managed centrally.

???+ webex "Register the Agentic App in the Developer Portal"

    1. Navigate to the [Webex Developer Portal](https://developer.webex.com).
    2. Log in with your Webex credentials.
    3. From the Home page, click on **Start Building Apps**.
    4. Then click **Create an Agentic App**. 
    5. Fill in the app details:
        - **Agentic App Module**: **MCP**
        - **Transport Type**: **Stremeable HTTP**
        - **Agentic App Name**: <copy>`Finance_MCP_[PARTNER_NAME]`</copy> Replace the PARTNER_NAME placeholder in the name. 
        - **Description**: <copy>`MCP Server providing stock market data, portfolio management, and order execution tools for the Webex Financial Group Investment Advisor.`</copy>
        - **Agentic App Icon**: Select any icon or upload your own icon. 
        - **Agentic App URL**: <copy>`https://mcp.cx-tme.com/investment/mcp`</copy>
        - **Agentic App auth type**: **API Key**

        ???+ warning "API Key Security"
            This API key is shared across all bootcamp participants for lab purposes only. In a production environment, each application would have its own unique API key. Never share API keys in public repositories or documentation.

    8. Click **Add Agentic App**.

    ???+ info "What happens during registration?"
        When you register the Agentic App, you can either keep the app for only use within your Webex tenant or submit it to the App Hub for other customers to find it within Control Hub. 

    ???+ gif "Register Agentic App"
        <figure markdown>
        ![Register Agentic App](./assets/lab4_register_agentic_app.gif)
        <figcaption>Registering the MCP Server in the Webex Developer Portal</figcaption>
        </figure>

---

## Lab 4.4 Provision the Agentic App in Control Hub

???+ Important
    For sections 4.3 and 4.4, you must use a **Customer Administrator** account rather than a Partner account. Partner accounts do not have the permissions required to access the **Agentic Apps** configuration in Control Hub, which is the only way to authorize and enable specific MCP tools for your AI Agent.

After registering the app in the Developer Portal, you need to provision it in **Control Hub** to control access and configure authentication for your organization.

???+ webex "Provision the Agentic App"

    1. From [Control Hub](https://admin.webex.com), navigate to **Apps** > **Agentic Apps**.
    2. You should see the **Webex Finance MCP Server** app in the list. Click on it.
    3. In the **General** tab:
        - Set the app to **Allowed** for your organization and click **Save**. 

    4. In the **Authentication** tab:
        - The authentication method should already be set to **API Key** based on your Developer Portal configuration.
        - Enter the API Key and click **Save**:

            <copy>`wxai_mcp_4c8232360c9d9e159b9018d9140ebbace155afa746703b3bd66261216a73ddad`</copy>

    5. In the **Tools** tab:
        - You should see the five tools exposed by the MCP Server:

            | Tool Name | Description |
            | :--- | :--- |
            | `get_stock_price` | Returns current market data for a given stock ticker |
            | `get_portfolio` | Retrieves investment positions for a given investment account |
            | `initiate_order` | Initiates a stock order and sends a payment link via email |
            | `confirm_order` | Confirms payment and writes the position to the portfolio |
            | `get_investment_account` | Checks the Investment ID using the Customer ID

        - **Enable** all five tools and click **Save**.
        - Optionally, toggle **Allow signature change** for each tool if you anticipate the MCP Server tools may be updated during the bootcamp.

        ???+ tip "Tool Governance"
            The Tools tab in Control Hub is where administrators can control which MCP tools are available to Webex AI Agents in the organization. This is a key governance feature — even if the MCP Server exposes 20 tools, the administrator can choose to enable only the ones relevant to the business use case.

    ???+ gif "Provision Agentic App in Control Hub"
        <figure markdown>
        ![Provision Agentic App](./assets/lab4_provision_agentic_app.gif)
        <figcaption>Configuring the Agentic App in Control Hub</figcaption>
        </figure>

---

### Lab 4.4.1 Understanding the MCP Server Tools

Before adding the MCP tools to your Webex AI Agent, it's important to understand what each tool does and how it interacts with your Airtable data.

The MCP Server is pre-deployed and shared across all participants. You do not need to host or modify it. It exposes five tools that the Investment Advisor will call during conversations. Each tool either **reads from** or **writes to** your Airtable tables using the Base ID and API Key you provide in the agent instructions.


#### Tool Overview

| Tool | Operation | Tables Involved |
| :--- | :--- | :--- |
| `get_investment_account` | Read | Customers → Investment |
| `get_stock_price` | Read (no Airtable) | None — uses hardcoded market data |
| `get_portfolio` | Read | Investment → Positions |
| `initiate_order` | Read (SELL) / External API (BUY) | Positions (SELL validation only) |
| `confirm_order` | Write | Positions |

---

#### Tool Details

???+ webex "`get_investment_account`"

    - **What it does:** Takes a Customer ID (e.g., `CUST-001`), looks it up in your **Customers** table, then finds the linked record in your **Investment** table.
    - **What it returns:** The Investment Account ID (e.g., `INV-001`).
    - **Airtable impact:** Read-only. Nothing is modified.

    **Data flow:**
    ```
    Customer ID (CUST-001) → Customers table → linked Investment record → Investment Account ID (INV-001)
    ```

???+ webex "`get_stock_price`"

    - **What it does:** Takes a stock ticker (e.g., `MOON`) and returns the current price, daily change, sector, and volatility.
    - **What it returns:** Market data for the requested ticker.
    - **Airtable impact:** None. Stock data is hardcoded on the server.

    **Available tickers:**

    | Ticker | Name | Sector | Price |
    | :--- | :--- | :--- | :--- |
    | MOON | MoonShotMeme | Tech | $42.69 |
    | YLO | YOLO-Tech | Tech | $8.05 |
    | DEBT | InfiniteDebtInc | Finance | $102.40 |
    | CATV | CatVideoStreaming | Entertainment | $15.20 |
    | HYPE | HypeTrainHoldings | Retail | $250.00 |

???+ webex "`get_portfolio`"

    - **What it does:** Takes an Investment Account ID, queries your **Investment** table, follows the linked records into your **Positions** table, and returns all positions.
    - **What it returns:** A list of positions, each with the Position ID, ticker, quantity, purchase price, and current price.
    - **Airtable impact:** Read-only. Nothing is modified.

    **Data flow:**
    ```
    Investment Account ID (INV-001) → Investment table → linked Positions records → list of positions
    ```

???+ webex "`initiate_order`"

    This tool behaves differently depending on whether the customer is **buying** or **selling**.

    === "BUY Flow"

        1. Calculates the total cost based on the current stock price and quantity.
        2. Creates a payment session with **NovaPay** (external payment provider).
        3. Triggers a webhook to send a payment link to the customer's email.
        4. Returns the payment session ID and URL.

        **Airtable impact:** None at this stage. The position is only written after payment is confirmed via `confirm_order`.

        **Data flow:**
        ```
        Order details → NovaPay API → payment session created → payment link emailed to customer
        ```

    === "SELL Flow"

        1. Looks up the customer's existing position for that ticker in your **Positions** table.
        2. Validates that the customer owns enough shares to sell.
        3. If the customer tries to sell more shares than they own, returns an error with the current holding.
        4. If validated, returns a confirmation prompt with the order details and projected proceeds.

        **Airtable impact:** Read-only at this stage. The position is only updated after the customer confirms via `confirm_order`.

        **Data flow:**
        ```
        Order details → Positions table lookup → ownership validated → confirmation prompt returned
        ```

???+ webex "`confirm_order`"

    This tool also behaves differently for **BUY** and **SELL**.

    === "BUY Confirmation"

        1. Checks the NovaPay payment status using the payment session ID.
        2. If payment is still pending, returns a pending status and asks the agent to try again later.
        3. If payment is completed, creates a **new row** in your **Positions** table with the Position ID, ticker, quantity, and purchase price.

        **Airtable impact:** A new record is **created** in the Positions table, linked to the Investment account.

        **Data flow:**
        ```
        Payment session ID → NovaPay status check → if completed → new Positions record created
        ```

    === "SELL Confirmation — Partial Sell"

        1. No payment check is performed.
        2. Finds the existing position in your **Positions** table.
        3. **Reduces the Quantity** on the existing record. For example, if the customer owns 10 shares and sells 3, the quantity is updated to 7.

        **Airtable impact:** The existing Positions record is **updated** (quantity reduced).

        **Data flow:**
        ```
        Sell 3 of 10 shares → Positions record updated → Quantity: 10 → 7
        ```

    === "SELL Confirmation — Full Sell"

        1. No payment check is performed.
        2. Finds the existing position in your **Positions** table.
        3. Since the customer is selling **all** shares, the position record is **deleted** from the table.

        **Airtable impact:** The existing Positions record is **deleted**.

        **Data flow:**
        ```
        Sell 10 of 10 shares → Positions record deleted → position closed
        ```

    ???+ tip "Partial vs. Full Sell"
        The server automatically determines whether to update or delete based on the remaining quantity. If `remaining = 0`, the record is deleted. Otherwise, only the quantity field is updated.

---

#### Required Airtable Fields

For the tools to work correctly, your **Positions** table must have the following fields:

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `Position_ID` | Text | Unique identifier (e.g., POS-001). Auto-generated by the server. |
| `Investment_Acct` | Link to Investment | Links the position to the investment account. |
| `Stock_Ticker` | Text | The stock ticker symbol (e.g., MOON). |
| `Quantity` | Number | Number of shares held. Updated on partial sells. |
| `Purchase_Price` | Number | Price per share at time of purchase. |
| `Current_Price` | Number | Current market price per share. Updated on sells. |

???+ warning "Field Name Accuracy"
    The MCP Server uses these exact field names when reading and writing to Airtable. If your field names do not match (e.g., `Stock Ticker` instead of `Stock_Ticker`), the tools will fail. Double-check your Airtable column names before testing.

## Lab 4.5 Connect the MCP Tools to the Investment Advisor

Now that the Agentic App is provisioned, you need to connect the MCP tools to your Investment Advisor AI Agent as actions.

???+ webex "Add MCP Tools as Actions"

    1. In the **AI Agent Studio**, select your *Investment Advisor Agent*.
    2. Select the **Actions** tab.
    3. Click on **+Add Actions** and use the **Select Available* option.

        ???+ info "MCP Action Type"
            Unlike Lab 2 where you selected **Fulfillment** (backed by Webex Connect flows), here you are going to select available tools. This tells the AI Agent that the action is fulfilled by an external MCP Server rather than a Webex Connect flow.

    5. You should see the five available tools. Select **get_stock_price** and click **Add**.
    6. Review the action configuration:
        - **Action name**: `get_stock_price`
        - The input schema (the `ticker` parameter) is automatically populated from the MCP Server's tool definition.
    7. Repeat the process for the remaining three tools:
        - Select **get_portfolio** and click **Add**.
        - Select **initiate_order** and click **Add**.
        - Select **confirm_order** and click **Add**.
        - Select **get_investment_account** and click **Add**.

    8. You should now see all five MCP actions in the **Actions** panel of your AI Agent.

    ???+ warning "Airtable Credentials"
        The `get_portfolio`, `initiate_order`, and `confirm_order` tools require your Airtable Base ID and API Key as input parameters. You need to instruct the AI Agent to use these values when calling these tools. Go back to the **Profile** tab and add the following to the **Instructions**:

        ```text
        **Airtable Configuration:**
        When calling tools that require airtable_base_id and airtable_api_key, use the following values:
        - airtable_base_id: YOUR_BASE_ID
        - airtable_api_key: YOUR_API_KEY
        ```

        Replace `YOUR_BASE_ID` and `YOUR_API_KEY` with your actual Airtable credentials.

        ???+ tip "Why pass credentials in the instructions?"
            In a production environment, credentials would be managed via secure environment variables or a secrets manager. For this bootcamp, embedding them in the instructions is the simplest approach that allows each partner to use their own Airtable base without modifying the shared MCP Server.

    9. Click **[Save changes]** and **Publish** the agent.

    ???+ gif "Add MCP Actions"
        <figure markdown>
        ![Add MCP Actions](./assets/lab4_add_mcp_actions.gif)
        <figcaption>Connecting MCP tools to the Investment Advisor</figcaption>
        </figure>

---

## Lab 4.6 Update the Call Flow for Agent-to-Agent Transfer

In Lab 3, you modified the flow to handle the `fraud_transfer` by routing to a human queue. Now you will add a third path in the **Case** node to handle the `investment_transfer` by routing to the Investment Advisor AI Agent.

???+ webex "Update the Flow"

    1. Go to **Control Hub** -> **Contact Center** -> **Flows**
    2. Open your flow **AI_Agent_DebtCollection** and go into **Edit** mode. 
    3. First, we need to create the flow variables that will hold the context from Alex. Click on the **Global Flow Properties** panel (the gear icon in the top-right of the flow editor).
    4. Under **Custom Flow Variables**, click the option **Create flow variable** and create the following variables:

        | Variable Name | Type | Default Value |
        | :--- | :--- | :--- |
        | <copy>`customer_id`</copy> | `String` | |
        | <copy>`customer_email`</copy> | `String` | |
        | <copy>`investment_id`</copy> | `String` | |

    3. Locate the **Case** node named `Transfer_Check`.
    4. Add a new case:
        - **Case 3** = <copy>`investment_transfer`</copy>

    5. Drag and drop a new **Parse** node onto the canvas.
    6. Connect the *investment_transfer* path of the **Case** node to the new **Parse** node.
    7. Click the **Parse** node and rename it to <copy>`Investment_Context`</copy>.
    8. In the Description, add: <copy>`Extracts investment transfer context from AI Agent metadata`</copy>
    9. Map the variables from the Virtual Agent MetaData:
        * **Input Variable** = `DebtCollection_Agent.MetaData`
        * **Content Type** = `JSON`

        | Output Variable | Path Expression |
        | :--- | :--- |
        | `customer_id` | <copy>`$.actions.investment_transfer[0].input.customer_id`</copy> |
        | `customer_email` | <copy>`$.actions.investment_transfer[0].input.customer_email`</copy> |
        | `investment_id` | <copy>`$.actions.investment_transfer[0].input.investment_id`</copy> |

        ???+ tip "Reusing the Pattern from Lab 3"
            This is the exact same pattern you used for the `fraud_transfer` in Lab 3.2. The only differences are the action name (`investment_transfer`) and the entity names. The MetaData JSON structure is consistent across all transfer actions.

    10. Drag and drop a **Virtual Agent V2** node onto the canvas.
    11. Connect the output of the **Investment_Context** Parse node to the new **Virtual Agent V2** node.
    12. Click on the **Virtual Agent V2** node and configure the settings:
        - **Activity Label**: <copy>`InvestmentAdvisor`</copy>
        - Under *Conversational Experience*, set the **Static Contact Center AI Config**
        - In the *Contact Center AI Config* select **Webex AI Agent (Autonomous)**
        - In the *Virtual Agent* drop down menu, select your **Investment Advisor Agent**.
        - In the *State Event* section, copy the below JSON in the *Event Data* field:

            ```json
            {
                "customer_id": "{{customer_id}}",
                "customer_email": "{{customer_email}}",
                "investment_id": "{{investment_id}}"
            }
            ```

            ???+ warning
                The variables on the right (`{{customer_id}}`, `{{customer_email}}`, `{{investment_id}}`) must match the flow variable names you defined in the **Parse** node. These are the variables that the Investment Advisor will receive as context from Alex.

    13. Connect the *Handled* outlet of the **Virtual Agent V2** node to the **End Flow** node.
    14. Connect the *Escalated* outlet to the **Generic_Queue** node (as a fallback if the Investment Advisor needs to escalate to a human).
    15. Connect the *Errored* outlet to the existing error **Play Message** node.
    16. Click **Validate**, then **Save and Publish** the flow.

    ???+ gif "Update Flow for Investment Transfer"
        <figure markdown>
        ![Update Flow](./assets/lab4_update_flow.gif)
        <figcaption>Adding the Investment Advisor path to the call flow</figcaption>
        </figure>

---

## Lab 4.7 Test the Complete Scenario :test_tube:

You can now test the full multi-agent flow from debt collection through investment advisory.

???+ webex "Preparation"
    Before testing, ensure the following:

    * [x] **Alex** has been published with the `investment_transfer` action (Lab 4.1).
    * [x] The **Investment Advisor** has been published with the five MCP actions (Lab 4.2 & 4.5).
    * [x] The **Agentic App** is registered and provisioned with all tools enabled (Lab 4.3 & 4.4).
    * [x] The **flow** has been published with the investment transfer path (Lab 4.6).
    * [x] Your **Airtable** has the Investment and Positions tables populated with test data.
    * [x] The **MCP Server** is running at `https://mcp.cx-tme.com/investment/mcp`.

???+ webex "Execute the Test"

    1. **Initiate the Call**: Trigger an outbound call via the Campaign Manager or call directly into your entry point.
    2. **Authenticate with Alex**: Complete the name verification and PIN authentication.
    3. **Trigger the Investment Transfer**: After authentication, say something like:

        <copy>"I'd like to check on my investment portfolio"</copy>

        or

        <copy>"Can I buy some stocks?"</copy>

    4. **Observe Alex's Behavior**: Alex should:
        - Recognize the investment intent.
        - Execute the `investment_transfer` action, passing your customer ID, email, and investment ID.
        - Inform you that you are being transferred to the Investment Advisor.

    5. **Interact with the Investment Advisor**: Once transferred, test the following scenarios:

        **Scenario A — Check Stock Price:**

        <copy>"What's the current price of MOON?"</copy>

        The advisor should call `get_stock_price` and return: ticker MOON, price $42.69, sector Tech, volatility Extreme.

        **Scenario B — Review Portfolio:**

        <copy>"Can you show me my current portfolio?"</copy>

        The advisor should call `get_portfolio` and return your positions from Airtable.

        **Scenario C — Place an Order:**

        <copy>"I want to buy 5 shares of CATV"</copy>

        The advisor should:
        - Confirm the order details (5 shares of CATV at $15.20 each = $76.00).
        - Call `initiate_order` to create a NovaPay payment session.
        - Inform you that a payment link has been sent to your email.
        - After you complete the payment, say: <copy>"I've completed the payment"</copy>
        - The advisor should call `confirm_order` to verify and finalize the order.
        - You should see a new position added to your Airtable Positions table.

???+ webex "Validation Checklist"

    | Step | Expected Result | Status |
    | :--- | :--- | :--- |
    | Alex recognizes investment intent | `investment_transfer` action triggers | ☐ |
    | Context passed to Investment Advisor | Customer ID, email, and investment ID available | ☐ |
    | `get_stock_price` works | Returns correct hardcoded market data | ☐ |
    | `get_portfolio` works | Returns positions from Airtable | ☐ |
    | `initiate_order` works | NovaPay session created, email sent | ☐ |
    | `confirm_order` works | Payment verified, position added to Airtable | ☐ |
    | New position visible in Airtable | Positions table has a new row | ☐ |

## Troubleshooting

???+ bug "General Troubleshooting"

    - **Transfer not triggering**: Verify that Alex's instructions include the `investment_transfer` logic and that the action is published. Check the AI Agent Sessions logs in the AI Agent Studio.
    - **MCP tools not appearing**: Ensure the Agentic App is set to **Allowed** in Control Hub and all five tools are **Enabled** in the Tools tab.
    - **Context variables empty after transfer**: Verify the Parse node path expressions match the entity names in the `investment_transfer` action. Use the flow debugger in Webex Connect to inspect the raw MetaData JSON.

???+ bug "Email Delivery Issues"

    The payment link email sent by NovaPay may land in the **spam/junk folder** due to the NovaPay domain reputation. The sender email is `wxccpartner@gmail.com`.

    **Recommended steps:**

    1. Check your **spam/junk folder** for the payment link email.
    2. If found in spam, mark it as "Not Spam" and add `wxccpartner@gmail.com` as a trusted sender/contact.
    3. If using Gmail, check the **Promotions** tab as well.
    4. **Verify the customer email** passed to `initiate_order` is correct. Check the AI Agent session logs to see the exact parameters sent.
    5. **NovaPay cold start**: The NovaPay service is hosted on Render's free tier and may take 30-60 seconds to wake up on the first request. If `initiate_order` returns an error, try again after a minute.
    6. **Webhook not configured**: If the MCP Server's `WEBHOOK_URL` environment variable is not set, the payment link will still be created (and returned in the tool response) but the email will not be sent. In this case, the AI Agent should still be able to relay the payment URL verbally or via chat.

    ???+ tip "Use Digital Preview Instead of Voice for Testing"
        When testing the payment flow, it is recommended to use the **AI Agent Preview (Digital)** option instead of voice. When using the voice channel, the AI Agent will prompt you every ~15 seconds with "Are you still there?" while you are waiting to receive and complete the payment email. The digital preview allows you to take your time completing the payment without being interrupted.

    <!-- Add screenshot of AI Agent Preview digital option here -->

???+ bug "Airtable Field and Table Name Errors"

    The MCP Server uses **exact field names and table names** when making API calls to Airtable. Mismatches will cause errors that may not be immediately obvious.

    **Common symptoms and causes:**

    | Error Code | Symptom | Likely Cause |
    | :--- | :--- | :--- |
    | `422` (Unprocessable Entity) | `get_investment_account` fails | Field values in your **Customers** table contain invalid or special characters (e.g., curly quotes, invisible Unicode characters in `CustomerID`). Re-type the values manually instead of pasting from formatted sources. |
    | `403` (Forbidden) | `get_portfolio` or `confirm_order` fails | The MCP Server is looking for a table named **`Investment`** but your Airtable base has it named **`Investments`** (or another variation). |
    | `404` (Not Found) | Any tool fails | A table or field referenced by the MCP Server does not exist in your Airtable base. |
    | `422` (Unprocessable Entity) | `confirm_order` fails on write | Field names in your **Positions** table don't match what the MCP Server expects (e.g., `Stock Ticker` instead of `Stock_Ticker`). |

    **How to verify your Airtable setup:**

    Run through the [Pre-Lab Check: Verifying Airtable Field Name Integrity](https://cx-partner.github.io/bootcamp2026/labs/pre_req_airtable/#pre-lab-check-verifying-airtable-field-name-integrity) to confirm your table names, field names, and field values are correct before troubleshooting further.

    <!-- Add screenshot showing correct Airtable table/field names here -->

    ???+ tip "Quick Validation"
        You can also test your Airtable setup independently by using the Airtable API directly in a browser or tool like Postman:
        ```
        GET https://api.airtable.com/v0/YOUR_BASE_ID/Investment
        Authorization: Bearer YOUR_API_KEY
        ```
        If this returns your records successfully, the table name and API key are correct. If you get a `403` or `404`, revisit the [Airtable Field Name Integrity check](https://cx-partner.github.io/bootcamp2026/labs/pre_req_airtable/#pre-lab-check-verifying-airtable-field-name-integrity).

???+ bug "Airtable Write Failures"

    If `confirm_order` fails when trying to write a new position or update an existing one:

    1. **Verify Airtable credentials**: Confirm that the `airtable_base_id` and `airtable_api_key` in your AI Agent instructions are correct and have write permissions.
    2. **Check field names and table names**: Run through the [Pre-Lab Check: Verifying Airtable Field Name Integrity](https://cx-partner.github.io/bootcamp2026/labs/pre_req_airtable/#pre-lab-check-verifying-airtable-field-name-integrity) to validate your Airtable structure.
    3. **Check linked record format**: The `Investment_Acct` field in the Positions table must be a **Link to another record** type pointing to the Investment table. If it's a plain text field, writes will fail with a 422 error.
    4. **Check API key permissions**: Ensure your Airtable Personal Access Token has `data.records:read` and `data.records:write` scopes for the correct base.

    <!-- Add screenshot showing Airtable API key permissions configuration here -->

???+ bug "Customer Administrator Account Required"

    Certain operations in this lab **require a Customer Administrator account**. A Partner account does not have the necessary permissions.

    **Operations that require Customer Admin:**

    | Operation | Why |
    | :--- | :--- |
    | Provisioning the Agentic App in Control Hub (Lab 4.4) | The **Apps > Agentic Apps** section is only accessible to Customer Admins |
    | Enabling MCP tools in Control Hub (Lab 4.4) | Tool enablement is an org-level admin function |
    | Selecting MCP actions in the AI Agent (Lab 4.5) | The "Select Available" option to discover MCP tools requires the org-level Agentic App to be provisioned, which is only visible to Customer Admins |

    **If you see any of the following symptoms, you are likely using the wrong account type:**

    - The **Apps** section in Control Hub does not show **Agentic Apps**.
    - The AI Agent **Actions > Select Available** shows no tools even though the Agentic App is registered in the Developer Portal.
    - You receive a permission error when trying to set the Agentic App to **Allowed**.

    ???+ warning "Action Required"
        If you are logged in with a Partner account, log out and log back in with your **Customer Administrator** credentials before proceeding with Labs 4.3 and 4.4. You can switch back to the Partner account for other lab sections.

    <!-- Add screenshot showing the difference between Partner and Customer Admin view in Control Hub here -->

???+ bug "MCP Server API Reference"

    The MCP Server is hosted at `https://mcp.cx-tme.com/investment/mcp` and exposes the following internal APIs. These are called automatically by the AI Agent through the MCP protocol — you do not need to call them directly. This reference is provided for troubleshooting purposes.

    **MCP Tools (called by the AI Agent):**

    | Tool | Method | Description |
    | :--- | :--- | :--- |
    | `get_investment_account` | MCP Tool Call | Resolves Customer ID → Investment Account ID via Airtable |
    | `get_stock_price` | MCP Tool Call | Returns hardcoded market data for a given ticker |
    | `get_portfolio` | MCP Tool Call | Reads Investment and Positions tables from Airtable |
    | `initiate_order` | MCP Tool Call | BUY: creates NovaPay session + sends email. SELL: validates ownership |
    | `confirm_order` | MCP Tool Call | BUY: checks payment + writes position. SELL: updates/deletes position |

    ---

    **External APIs called by the MCP Server:**

    | API | URL | Purpose |
    | :--- | :--- | :--- |
    | Airtable API | `https://api.airtable.com/v0/{base_id}/{table}` | Reads and writes customer, investment, and position data |
    | NovaPay Create Session | `https://novapay-moeh.onrender.com/api/create-session` | Creates a payment session for BUY orders |
    | NovaPay Session Status | `https://novapay-moeh.onrender.com/api/session-status` | Checks payment completion status for BUY confirmation |
    | Webhook (Email) | Configured via environment variable | Sends payment link email to customer |

    ---

    ### Airtable API Calls by Tool

    All Airtable requests use the following headers:
    ```
    Authorization: Bearer {airtable_api_key}
    Content-Type: application/json
    ```

    ---

    **`get_investment_account`** — Airtable Calls:

    *Step 1 — Look up customer record:*
    ```
    GET https://api.airtable.com/v0/{base_id}/Customers?filterByFormula={CustomerID}="CUST-001"&maxRecords=1
    ```
    Returns the Airtable record ID for the customer (e.g., `rec1abc123`).

    *Step 2 — Find linked investment account:*
    ```
    GET https://api.airtable.com/v0/{base_id}/Investment
    ```
    Iterates through all Investment records and matches where the `CustomerID` linked field contains the customer record ID from Step 1. Returns the `Investment_Acct` value (e.g., `INV-001`).

    ---

    **`get_portfolio`** — Airtable Calls:

    *Step 1 — Look up investment record:*
    ```
    GET https://api.airtable.com/v0/{base_id}/Investment?filterByFormula={Investment_Acct}="INV-001"
    ```
    Returns the Investment record, including the `Positions` field (an array of linked Positions record IDs).

    *Step 2 — Fetch each linked position:*
    ```
    GET https://api.airtable.com/v0/{base_id}/Positions/{record_id}
    ```
    Called once per linked position record ID. Returns `Position_ID`, `Stock_Ticker`, `Quantity`, `Purchase_Price`, and `Current_Price`.

    ---

    **`initiate_order` (SELL only)** — Airtable Calls:

    *Step 1 — Look up investment record:*
    ```
    GET https://api.airtable.com/v0/{base_id}/Investment?filterByFormula={Investment_Acct}="INV-001"
    ```

    *Step 2 — Find matching position by ticker:*
    ```
    GET https://api.airtable.com/v0/{base_id}/Positions/{record_id}
    ```
    Called for each linked position until one matches the requested `Stock_Ticker`. Returns the current `Quantity` to validate the customer owns enough shares.

    ???+ info "BUY orders do not call Airtable"
        For BUY orders, `initiate_order` only calls the NovaPay API (see below). No Airtable reads or writes happen until `confirm_order`.

    ---

    **`confirm_order` (BUY)** — Airtable Calls:

    *Step 1 — Get next Position ID:*
    ```
    GET https://api.airtable.com/v0/{base_id}/Positions?fields[]=Position_ID&sort[0][field]=Position_ID&sort[0][direction]=desc&maxRecords=1
    ```
    Returns the highest existing `Position_ID` (e.g., `POS-003`) so the server can generate the next one (`POS-004`).

    *Step 2 — Create new position record:*
    ```
    POST https://api.airtable.com/v0/{base_id}/Positions
    ```
    Request body:
    ```json
    {
        "fields": {
            "Position_ID": "POS-004",
            "Investment_Acct": ["rec_investment_record_id"],
            "Stock_Ticker": "CATV",
            "Quantity": 5,
            "Purchase_Price": 15.20,
            "Current_Price": 15.20
        },
        "typecast": true
    }
    ```

    ---

    **`confirm_order` (SELL — Partial)** — Airtable Calls:

    *Step 1 — Find existing position (same as `initiate_order` SELL):*
    ```
    GET https://api.airtable.com/v0/{base_id}/Investment?filterByFormula={Investment_Acct}="INV-001"
    GET https://api.airtable.com/v0/{base_id}/Positions/{record_id}
    ```

    *Step 2 — Update position quantity:*
    ```
    PATCH https://api.airtable.com/v0/{base_id}/Positions/{record_id}
    ```
    Request body:
    ```json
    {
        "fields": {
            "Quantity": 7,
            "Current_Price": 42.69
        }
    }
    ```
    Example: Customer owned 10 shares, sold 3, new quantity is 7.

    ---

    **`confirm_order` (SELL — Full)** — Airtable Calls:

    *Step 1 — Find existing position (same as above):*
    ```
    GET https://api.airtable.com/v0/{base_id}/Investment?filterByFormula={Investment_Acct}="INV-001"
    GET https://api.airtable.com/v0/{base_id}/Positions/{record_id}
    ```

    *Step 2 — Delete position record:*
    ```
    DELETE https://api.airtable.com/v0/{base_id}/Positions/{record_id}
    ```
    The position is fully closed and removed from the table.

    ---

    ### NovaPay API Details

    *Create Session* (`POST /api/create-session`):
    ```
    POST https://novapay-moeh.onrender.com/api/create-session
    ```
    Request body:
    ```json
    {
        "amount": 76.00,
        "customerEmail": "customer@example.com",
        "agentId": "session-id-from-ai-agent"
    }
    ```
    Returns:
    ```json
    {
        "sessionId": "nova_sess_abc123",
        "paymentUrl": "https://novapay-moeh.onrender.com/pay/nova_sess_abc123"
    }
    ```

    *Session Status* (`GET /api/session-status`):
    ```
    GET https://novapay-moeh.onrender.com/api/session-status?sessionId=nova_sess_abc123
    ```
    Returns:
    ```json
    {
        "status": "completed",
        "confirmationCode": "CONF-7891",
        "last4": "4242"
    }
    ```

    Possible `status` values: `pending`, `completed`, `expired`, `failed`

    ???+ tip "Testing APIs Independently"
        If a tool is failing, you can reproduce the exact Airtable or NovaPay API call using Postman, curl, or your browser to isolate whether the issue is with the MCP Server or with your Airtable configuration.

        **Airtable example:**
        ```bash
        curl -H "Authorization: Bearer YOUR_API_KEY" \
             "https://api.airtable.com/v0/YOUR_BASE_ID/Investment"
        ```

        **NovaPay example:**
        ```bash
        curl "https://novapay-moeh.onrender.com/api/session-status?sessionId=YOUR_SESSION_ID"
        ```

        If Airtable returns a `403` or `404`, revisit the [Airtable Field Name Integrity check](https://cx-partner.github.io/bootcamp2026/labs/pre_req_airtable/#pre-lab-check-verifying-airtable-field-name-integrity). If NovaPay returns a timeout or 503, wait 30-60 seconds for the service to wake up (Render free tier cold start) and retry.