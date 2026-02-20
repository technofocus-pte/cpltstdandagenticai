# Lab 4 - Build an autonomous financial data retrieval agent with Computer-Using Agents (CUA)

**Introduction**

Legacy systems without APIs create major roadblocks for automation.
Traditional RPA often relies on fragile screen-scraping or manual
workarounds, which slow down decision-making, increase errors, and
reduce productivity. This lab introduces Microsoft Copilot Studio and
Computer Using Agents (CUA) as a smarter solution. By simulating human
interaction with internal systems, CUAs can securely access and process
data - without needing API integration. You’ll learn to build an
autonomous agent that delivers faster responses, reduces manual
workload, and enables real-time, informed decisions.

Objective

In this lab, you’ll learn how to build an autonomous agent using
Microsoft Copilot Studio. This agent will simulate human interaction
with a legacy internal system to retrieve financial portfolio data
without requiring direct API access.

## Task 1: Create and Configure an Autonomous Agent

In this task, you will create a new autonomous agent in Microsoft
Copilot Studio, configure its identity, and set up an email trigger
using the Microsoft 365 Outlook connector.

To automate portfolio lookups, the agent must be able to detect incoming
email requests and initiate the appropriate automation flow based on
subject line filtering.

1.  Login to the Copilot Studio at
    +++https://copilotstudio.microsoft.com+++ using your login
    credentials.

2.  Select the **Dev One** environment from the top right.

    ![](./media/image1.png)

3.  Select **Create an agent**.

    ![](./media/image2.png)

4.  Once the agent is created, select **Edit** against the **Details**.

    ![](./media/image3.png)

5.  Enter the Name as +++Portfolio Lookup Agent+++ and select Save to
    rename the default name of the agent.

    ![](./media/image4.png)

6.  Scroll down to the triggers section and click **+Add trigger**.

    ![](./media/image5.png)

7.  Search and select **When a new email arrives (V3) (Office 365
    Outlook** and click on **Next**. 

    ![](./media/image6.png)

8.  Rename the trigger to +++When a portfolio lookup email arrives+++,
    ensure that the connection is established for Copilot Studio and
    Outlook and then click on **Next**.

    ![](./media/image7.png)

9.  In the **Subject Filter (Optional)** field, enter +++Portfolio+++ in
    the subject line.

    ![](./media/image8.png)

10. Once the trigger is created, you can **Close** the Time to test your
    trigger dialog.

    ![](./media/image9.png)

## Task 2: Add Computer Use tool 

In this task, you will configure a Computer use tool that logs into a
computer, navigates through a website, searches and retrieves financial
portfolio data. Then use the Office 365 Outlook connector to reply with
the requested data.

1.  Navigate to **Tools** in the top-level menu.

    ![](./media/image10.png)

2.  Select **+ Add a tool.**

    ![](./media/image11.png)

3.  Select **+ New tool**.

    ![](./media/image12.png)

4.  Select **Computer use (preview)**.

    ![](./media/image13.png)

5.  Add the following Instructions, and then select **Add and
    configure**.

    ```
    1.  Go to https://computerusedemos.blob.core.windows.net/web/Portfolio/index.html.
    
    2.  Enter the Portfolio ID in the "Enter Portfolio ID" search field and click on the "Search" button.
    
    3.  Retrieve the "Client Name", "Portfolio Value" and "Manager" values exactly as shown.
    
    4.  Return those three values as the final output. If no portfolio data is found, reply that you couldn't find a portfolio with the specified ID.
    ```
    
    ![](./media/image14.png)

6.  Update the **Name** of the Computer use tool as +++Look up portfolio
    data+++

7.  Update the **Description** as +++Search and retrieve financial portfolio data+++

    ![](./media/image15.png)

8.  In the Inputs section select **+ Add input**.

    ![](./media/image16.png)

9.  Enter name as +++Portfolio ID+++ and description +++The ID of the portfolio+++ and select **Done**.

    ![](./media/image17.png)

10. Select **Save**.

    ![](./media/image18.png)

## Task 3: Test the Computer use tool

1.  In the **Instructions** section, select the **Test** button on the
    right.

    ![](./media/image19.png)

2.  Add the sample value +++44123BCD+++ and select **Test now**.

    ![](./media/image20.png)

3.  Observe the Computer use tool logging into the computer and
    performing the requested actions:

    - The left panel shows your instructions and a step-by-step log of
      the tool’s reasoning and actions.

    - The right panel shows a preview of the actions on the machine you
      set up for computer use.

    ![](./media/image21.png)

    ![](./media/image22.png)

    ![](./media/image23.png)
    
    ![](./media/image24.png)
    
    ![](./media/image25.png)
    
    ![](./media/image26.png)

4.  Select **Finish testing**.

    ![](./media/image27.png)

## Task 4: Setting up email response capabilities

In this task, you will set up the email capability.

1.  Return to the **Tools** tab and select **+ Add a tool** .

    ![](./media/image28.png)

2.  Search for +++**Send an email (V2) (Office 365 Outlook)**+++ and
    select it.

    ![](./media/image29.png)

3.  Select **Add and configure**.

    ![](./media/image30.png)

4.  Update its **Name** to +++Reply to email+++ and **Description** to,
    +++Use this operation to reply to the email received+++ and then
    select **Additional details**.

    ![](./media/image31.png)

5.  Under **Additional details**, set **Credentials to use** to
    **Maker-provided credentials.**

    ![](./media/image32.png)

6.  Under the **Inputs** section, click on **customize** against the
    **To** input and set its **Description** to +++Use the "from" email of the triggering received email+++.

    ![](./media/image33.png)
    
    ![](./media/image34.png)

7.  **Customize** the **Subject** input and set its **Description** to
    +++Write the email subject+++.

    ![](./media/image35.png)

8.  Customize the **Body** input and set its **Description** to +++Write
    the email body using HTML and highlight the requested data+++.

    ![](./media/image36.png)

9.  Click **Save** to finalize the tool configuration.

    ![](./media/image37.png)

10. Navigate to **Overview** tab and then **Edit** the Instructions.

    ![](./media/image38.png)

11. Paste the following instruction.

    ```
    When a financial portfolio related request is received, identify the Portfolio ID and search for the requested data using < Look up portfolio data >. Once you have gathered the financial portfolio information, use the < Reply to email > tool to reply to the original email you received. Do not respond with data beyond what was requested.
    ```
    
    ![](./media/image39.png)

12. Select < Look up portfolio data >, enter / and select the **tool**
    **Look up portfolio data**.

    ![](./media/image40.png)

    ![](./media/image41.png)

13. Similarly, replace < Reply to email > with the **tool**, **Reply to
    email**.

14. Once the replacements are done, as in the screenshot below, select
    **Save**.

    ![](./media/image42.png)

15. Select **Settings** from the top right.

    ![](./media/image43.png)

16. **Disable** **Use general knowledge option** under the **Knowledge**
    section, and select **Save**.

    ![](./media/image44.png)

17. Close the **Settings** pane.

    ![](./media/image45.png)

## Task 5: Testing your complete agent

In this agent, you will test the complete working of the agent that you
have created.

1.  Send a test email from an email address of your preference to your
    training user’s email account with

    Subject: +++Portfolio data request+++
    
    Body:

    ```
    Hi! 
    I hope you're doing well! 
    I'm looking for the portfolio manager and value of portfolio #44123BCD. Much appreciated. 
    
    Thanks!
    ```

    ![](./media/image46.png)

2.  Make sure you receive the email in your training user’s inbox.

3.  In the **Overview** tab, go to the **Triggers** section and select
    **Test trigger**.

    ![](./media/image47.png)

4.  Select the **trigger instance** and then **Start testing.**

    ![](./media/image48.png)

5.  The execution happens and you can see the updates and the flow in
    the Test pane.

    ![](./media/image49.png)

    ![](./media/image50.png)

6.  Once the execution is completed, check your email for the agent’s
    reply.

    ![](./media/image51.png)

## Summary

In this lab, you built an autonomous financial data retrieval agent
using Microsoft Copilot Studio and Computer-Using Agents (CUA). You
configured an event-driven agent that automatically responds to email
requests, simulates human interaction with a legacy system to retrieve
portfolio data, and returns accurate results without relying on APIs.

You learned how to:

- Design an autonomous agent that operates without direct user
  interaction

- Use email-based triggers to initiate automated workflows

- Configure Computer-Using Agents to securely navigate and extract data
  from legacy web applications

- Integrate action tools to return results via email

- Reduce reliance on fragile RPA patterns by using AI-driven computer
  interaction

This lab demonstrates how autonomous agents with CUA can modernize
legacy system access, streamline operational workflows, and enable
faster, more reliable decision-making in environments where APIs are
unavailable.
