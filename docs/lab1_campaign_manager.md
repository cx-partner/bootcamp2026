# Lab X.X - [Insert Lab Title Here] :emoji:

[Provide a high-level summary of what the user will achieve in this lab. Mention the tools involved (e.g., Control Hub, Webex Connect, Bruno).]

???+ purpose "Lab Objectives"
    * Objective 1: Describe the outcome.
    * Objective 2: Describe the technical integration.

???+ warning "Important Note"
    Insert critical warnings here (e.g., "Do not use your personal email," or "Ensure you are using the VPN").

---

## Lab X.1 [Sub-section Title]

[Briefly explain the context of this specific task.]

???+ webex "Instructions"
    1. Navigate to **[Menu Name]** > **[Sub-menu]**.
    2. Click the **[Button Name]** button.
    3. Enter the following parameters:
        - **Name**: `PODXX_Task_Name` (Replace XX with your POD ID)
        - **Type**: Select `Option A`
        - **Description**: "Test description"
    4. Click **Save**.

    ???+ info "Visual Reference: [Description] IMG"
        <figure markdown>
        ![Alt Text](./assets/your_image_name.png)
        <figcaption>Optional: Description of the screenshot</figcaption>
        </figure>

## Lab X.2 [Configuration Task]

!!! note "Documentation Link"
    You can find the official documentation [here](https://link-to-docs.com).

???+ webex "Step-by-Step Configuration"
    1. Open the **[Tool Name]**.
    2. Locate your resource and update the values as shown in the table below:

    | Parameter | Value | Notes |
    | :--- | :--- | :--- |
    | **Variable A** | `$(n2.Value)` | Use the node ID from your flow |
    | **Variable B** | `67e2e90e...` | Provided Lab ID |

    3. For JSON configurations, use the snippet below:
    ```json
    {
      "id": "$(n2.aiAgent.transId)",
      "status": "active",
      "data": {
        "pod": "XX"
      }
    }
    ```

    ???+ tip "Pro-Tip"
        If you encounter an error during validation, ensure there are no trailing spaces in your URL.

---

## Testing :test_tube:

[Explain how the user can verify their work is correct.]

???+ webex "Validation Steps"
    1. **Trigger the event**: [Describe how to trigger it, e.g., "Place a call to..."]
    2. **Observe the result**: [Describe what should happen, e.g., "You should see a pop-up..."]
    3. **Check logs**: Navigate to the **Logs** tab and verify the status is `200 OK`.

---

## Conclusion
**Congratulations!** You have successfully completed Lab X.X. You are now ready to move on to the next section.

[Optional: Link to next lab]
[Next Lab: Lab Y.Y - Title](./lab_y_y.md){ .md-button .md-button--primary }