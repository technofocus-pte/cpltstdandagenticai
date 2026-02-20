# Lab 3 - Transforming the hiring agent into a scalable multi-agent architecture

In the earlier lab, you built your main Hiring Agent giving you a solid
foundation for managing recruitment workflows. But one agent can only do
so much.

Your assignment, should you choose to accept it, is **Operation
Symphony** - transforming your single agent into a **multi-agent
system**: an orchestrated team of specialized agents that work together
to handle complex hiring challenges. Think of it as upgrading from a
solo operator to commanding a specialized task force.

Like a symphony orchestra where each musician plays their part in
perfect harmony, you'll add two critical specialists to your existing
Hiring Agent: an Application Intake Agent to process resumes
automatically, and an Interview Prep Agent to create comprehensive
interview materials. These agents will work together seamlessly under
your main orchestrator.

After creating multi agents, you'll transform your agents from waiting
for human input to proactively responding to external events and taking
intelligent action without supervision.

Think of it as upgrading from agents that *answer questions* to agents
that *anticipate needs* and *act independently*. Through event triggers
and automated workflows, your Hiring Agent will detect incoming resume
emails, process attachments automatically, store data in Dataverse, and
notify your HR recruitment team via Microsoft Teams - all while you
focus on higher-value tasks.

## Objectives

In this mission, you'll learn:

-    When to use **child agents** vs **connected agents**

-    How to design **multi-agent architectures** that scale

-    Creating **child agents** for focused tasks

-    Establishing **communication patterns** between agents

-    Building the Application Intake Agent and Interview Prep Agent

-    How event triggers enable autonomous agent behavior without user
    interaction

-    The differences between interactive and autonomous agents in Copilot
    Studio

-    How to create event triggers that automatically process email
    attachments and upload files to Dataverse

-    How to build agent flows that post adaptive cards to Teams channels
    for notifications

-    How to pass data between event triggers and agent flows for
    end-to-end automation

## Child agent: Application Intake Agent

Let's start building our multi-agent hiring system. Our first specialist
will be the **Application Intake Agent** - a child agent responsible for
processing incoming resumes and candidate information.

![](./media/image1.png)

**Application Intake Agent responsibilities**

- **Parse resume content** from PDFs provided via interactive chat (In a
  future mission you'll learn how to process resumes autonomously).

- **Extract structured data** (name, skills, experience, education)

- **Match candidates to open roles** based on qualifications and cover
  letter

- **Store candidate information** in Dataverse for later processing

- **Deduplicate applications** to avoid creating the same candidate
  twice, match to existing records using the email address extracted
  from the resume.

**Why this should be a child agent**

The Application Intake Agent fits perfectly as a child agent because:

- It's specialized for document processing and data extraction

- It doesn't need separate publishing

- It's part of our overall hiring solution managed by the same team

- It focuses on a specific trigger (new resume received) and is invoked
  from the Hiring Agent.

## Connected agent: Interview Prep Agent

Our second specialist will be the **Interview Prep Agent** - a connected
agent that helps create comprehensive interview materials and evaluates
candidate responses.

**Interview Prep Agent responsibilities**

- **Create interview packs** with company information, role
  requirements, and evaluation criteria

- **Generate interview questions** tailored to specific roles and
  candidate backgrounds

- **Answer general questions** about the job roles and applications for
  stakeholder communication

**Why this should be a connected agent**

The Interview Prep Agent works better as a connected agent because:

- The talent acquisition team might want to use it independently across
  multiple hiring processes

- It needs its own knowledge base of interview best practices and
  evaluation criteria

- Different hiring managers might want to customize its behavior for
  their teams

- It could be reused for internal positions, not just external hiring

## Exercise 1 - Adding the Application Intake Agent

Let's add our first child agent to your existing Hiring Agent.

### Task 1 - Solution setup

1.  Inside Copilot Studio, select the ellipsis (...) below Tools in the
    left hand navigation.

2.  Select **Solutions**.

    ![](./media/image2.png)

3.  Locate your **Operative** solution, select the **ellipsis
    (...)** next to it, and choose **Set preferred solution**.
    Select **Apply** in the dialogue box that pops up. This will ensure
    that all your work will be added to this solution.

    ![](./media/image3.png)

4.  Select Apply in the Set your preferred solution dialog box.

    ![](./media/image4.png)

### Task 2 - Configure your Hiring Agent instructions

1.  **Navigate** to Copilot Studio. Ensure that the environment **Dev One** is selected
    in the top right **Environment Picker**.

2.  Open the **Hiring Agent**.

3.  Select **Edit** in the **Instructions** section of
    the **Overview** tab of the agent.

    ![](./media/image5.png)

4.  Copy and paste the following instructions in the instructions input area.

    +++You are the central orchestrator for the hiring process. You coordinate activities, provide summaries, and delegate work to specialized agents.+++

5.  Select **Save**.

    ![](./media/image6.png)

6.  Select the **Settings** button in the top right of the screen.

    ![](./media/image7.png)

7.  Review the page and ensure the following settings are applied and
    then select **Save**.

    - Use generative AI orchestration for your agent's responses - **Yes**
    - Deep Reasoning - **Off**
    - Let other agents connect to and use this one - **On**
    - Continue using retired models - **Off**
    - Content Moderation - **Moderate**
    - Collect user reactions to agent messages - **On**
    - Use general knowledge - **Off**
    - Use information from the Web - **Off**
    - File uploads - **On**
    - Code Interpreter - **Off**

    ![](./media/image8.png)

    ![](./media/image9.png)

    ![](./media/image10.png)

    ![](./media/image11.png)

9.  Once the changes are saved, click the **X** in the upper right hand corner to close out of the
    settings menu

     ![](./media/image12.png)

### Task 3 - Add the Application Intake child agent

In this task, you will add a child agent to the Hiring agent.

1.  **Navigate** to the **Agents** tab within your Hiring Agent (this is
    where you'll add specialist agents) and select **Add**.

    ![](./media/image13.png)

2.  Select **New child agent**.

    ![](./media/image14.png)

3.  **Name** your agent +++Application Intake Agent+++

4.  Select **The agent chooses - Based on description** in the **When
    will this be used?** dropdown. These options are similar to the
    triggers that can be configured for topics.

5.  Set the **Description** to be - +++Processes incoming resumes and stores candidates in the system+++

    ![](./media/image15.png)

6.  Expand **Advanced**, and set the Priority to be 10000. This will
    ensure that later the Interview Agent will be used to answer general
    questions before this one. A condition could be set here as well
    such as ensuring that there is at least one attachment.

    ![](./media/image89.png)

7.  Ensure that the toggle **Web Search** is set to **Disabled**. This
    is because we only want to use information provided by the parent
    agent. Select **Save**

    ![](./media/image17.png)

### Task 4 - Configure Resume Upload agent flow

Agents can't perform any actions without being given tools or topics.

We're using **Agent Flow tools** rather than Topics for the *Upload
Resume* step because this multi-step backend process requires
deterministic execution and integration with external systems. While
Topics are best for guiding the conversational dialog, Agent Flows
provide the structured automation needed to reliably handle file
processing, data validation, and database upserts (insert new or update
existing) without depending on user interaction.

1.  Locate the **Tools** section inside the Application Intake Agent
    page. 

    >[!Alert] **Important:** This isn't the Tools tab of the parent agent, but can be found if you scroll down underneath the child agent instructions.

2.  Select **+ Add**.

    ![](./media/image18.png)

3.  Select **+ New tool**.

    ![](./media/image19.png)

4.  Select **Agent flow**. The Agent Flow designer will open, this is
    where we will add the upload resume logic.  

    ![](./media/image20.png)

    >[!Alert] Important: If **+ New tool** option is not available and **Agent Flow** is directly available, then please select Agent flow.
    >
    >![](./media/image90.png)

6.  Select the **When an agent calls the flow** node, and select **+ Add
    an input**

     ![](./media/image21.png)

7.  Add **inputs**. Select the appropriate input type as shown in the table
    and be sure to add both the name and the description. It's important
    to include the description because it will help the agent know what
    to fill in the input.

    | **Type**   |  **Name**  |  **Description**  |
    |:----|:-------|:-----|
    |  File  | +++Resume+++   |  +++The Resume PDF file+++  |
    | Text   |  +++Message+++  |  +++Extract a cover letter style message from the context. The message must be less than 2000 characters.+++  |
    | Text   | +++UserEmail+++   |  +++The email address that the Resume originated from. This will be the user uploading the resume in chat, or the from email address if received by email.+++  |
    
    ![](./media/image22.png)

8.  Select the **+ icon** below the when an agent calls the flow node
    and search for +++Dataverse add+++, then select the **Add a new
    row** action in the **Microsoft Dataverse** section.

    ![](./media/image23.png)

    ![](./media/image24.png)

    >[!Note] **NOTE:** You may be prompted to create a new connection to Dataverse after you
    add the action. Enter any **name** for the connection and click **Signin** and follow the prompts to
    create that connection.
    >
    >![](./media/image91.png)
    >
    >![](./media/image92.png)
    >
    >If you face issues in creating connection due to popup blocker as in the screenshot below, please disable the popup blocker to proceed with the connection creation.
    >
    >![](./media/image93.png)

8.  Name the node +++**Create Resume**+++, by selecting the 3 dot and
    select **Rename**.  

    ![](./media/image25.png)

9.  Set the **Table name** to **Resumes**, then select **Show all**, to
    show all the parameters.

    ![](./media/image26.png)

10. Set the following **properties**:

    |  **Property**  | **How to Set**   | **Details / Expression**   |
    |:----|:------|:-----|
    |   **Resume Title** | Dynamic data (thunderbolt icon)   | **When an agent calls the flow → Resume name** If you don't see the Resume name, make sure you have configured the Resume parameter above as a data type.  |
    |  Cover letter  | Expression (fx icon)   | +++if(greater(length(triggerBody()?['text']), 2000), substring(triggerBody()?['text'], 0, 2000), triggerBody()?['text'])+++ Click on **Add** after the expression is entered.  |
    |  **Source Email Address**  |Dynamic data (thunderbolt icon)   | **When an agent calls the flow → UserEmail**   |
    |  **Upload Date**  | Expression (fx icon)   |  +++utcNow()+++ Click on **Add** after the expression is entered.  |

    ![](./media/image27.png)

    ![](./media/image28.png)

    ![](./media/image29.png)

12. Select the **+ icon** below the Create Resume node, search
    for +++Dataverse upload+++ and select the **Upload a file or an
    image** action.

    ![](./media/image30.png)

13. Name the node to +++**Upload Resume File**+++.

    ![](./media/image31.png)

14. Set the following **properties**:

    |  **Property** |  **How to Set**  |  **Details**  |
    |:--------|:---------|:---------|
    | **Content name**   | Dynamic data (thunderbolt icon)   | When an agent calls the flow → Resume name   |
    | **Table name**   |  Select  |  Resumes  |
    |  **Row ID**  |  Dynamic data (thunderbolt icon)  | Create Resume → See more → Resume   |
    |  **Column Name**  |   Select |  Resume PDF  |
    | **Content**   |  Dynamic data (thunderbolt icon)  | When an agent calls the flow → Resume contentBytes   |
    

    ![](./media/image32.png)

16. Select the **Respond to the agent node**, and then select **+ Add an
    output**. Create an output with the properties defined in the table
    below.

    ![](./media/image33.png)

     | **Property**   |  **How to Set**  |  **Details**  |
     |:-----|:--------|:--------|
     |  **Type**  |  Select  |  Text  |
     |  **Name**  |  Enter  | +++ResumeNumber+++   |
     |  **Value**  |  Dynamic data (thunderbolt icon)  |  Create Resume → See More → Resume Number  |
     |  **Description**  |  Enter  | +++The [ResumeNumber] of the Resume created+++   |
    
    ![](./media/image34.png)

18. Select **Save draft** on the top right

    ![](./media/image35.png)

19. Select the **Overview** tab, Select **Edit** on
    the **Details** panel. Fill in the name and description as shown
    below and select **Save**

    -  **Flow name**:+++Resume Upload+++

    -  **Description**:+++Uploads a Resume when instructed+++

    ![](./media/image36.png)

20. Select the **Designer** tab again and select **Publish**.

    ![](./media/image37.png)

### Task 5 - Connect the flow to your agent

Now you'll connect the published flow to your Application Intake Agent.

1.  Navigate back to the **Hiring Agent** and select the **Agents** tab.
    Open the **Application Intake Agent**, locate the **Tools** panel
    and select **+Add**.  
    ![](./media/image38.png)

2.  Select the **Flow** filter and select the **Resume Upload** flow.

    ![](./media/image39.png)

3.  Select **Add and configure**.

    ![](./media/image40.png)

4.  Set the following parameters for the **description** and **when the
    tool should be used**.

    **Description** - +++Uploads a Resume when instructed. STRICT RULE: Only call this tool when referenced in the form "Resume Upload" and there are Attachments+++
    
    **Additional details** → **When this tool may be used** - only when referenced by topics or agents
    
    ![](./media/image41.png)

    **Note:** This description tells the agent when it should call this tool. Notice the use of "strict rule" in the description. This gives a way to provide additional guardrails on when the tool should be used,  in this case, only if there are attachments and the context of the conversation is a resume upload. Choosing when this tool can be used is important as well. Since we are building a multi-agent system and we have a child agent, we want to be sure this tool is ONLY called in the child agent, not the main agent. Setting tha value to "only when referenced by topics or agents" ensure this.

6.  Scroll down to the inputs section and select **Add Input** to add
    the following inputs:

    Inputs → Add Input - **contentBytes**

    Inputs → Add Input - name

    ![](./media/image42.png)

8.  Now we need to set the properties of the inputs. We'll start with
    the **contentBytes** input which will store the actual resume file.
    Select **Custom value** from the **Fill using** dropdown next to
    the **contentBytes** input. In the **Value** property, select
    the **three dots (...)**.

    ![](./media/image43.png)

9.  Select the **Formula** tab. Paste in the following formula which
    extracts the file from the chat and click the **Insert** button.

    +++First(System.Activity.Attachments).Content+++

    ![](./media/image44.png)

10.  Now we'll configure the **name** input which will store the name of
    the resume file. This will be hard coded as well so select
    the **Custom value** option in the **Fill using** column.

11. Select the **three dots (...)** in the **Value** column and paste in
    the following formula which extracts the file name from the chat and
    click the **Insert** button.

    +++First(System.Activity.Attachments).Name+++

    ![](./media/image45.png)

11. Now we'll configure the **Message** input. We want to fill this one
    dynamically with AI so we'll leave the fill using as-is. Select
    the **Customize** button in the **Value** column so we can fill out
    additional details for how this should be filled.

    ![](./media/image46.png)

12. Enter the following in the **Description** field for the input. Then
    select **Advanced**.

    +++Extract a cover letter style message from the context. Be sure to never prompt the user and create at least a minimal cover letter from the available context. STRICT RULE - the message must be less than 2000 characters.+++

    **NOTE** Filling in the description for your dynamically filled inputs is a
crucial step to ensure that your agent knows how to fill in the input
correctly.

    ![](./media/image47.png)

13. Expand out the **Advanced** section to configure some additional
    properties for this input. In the **How many reprompts** section,
    select **Don't repeat**

    ![](./media/image48.png)

    **NOTE**
    
    This setting helps you customize your user experience so the agent
    doesn't ask the same question multiple times if it can't identify the
    data it needs.

14. Scroll down to the **No valid entity found** section. Select
    the **Set variable to value** option in the **Action if no entity
    found** dropdown. Type +++Resume upload+++ in the **Default entity
    value** input.

    ![](./media/image49.png)

    **NOTE**

    This setting lets us hard code a backup value if the agent is unable to dynamically fill this message input.

15. We'll fill the **UserEmail** input by selecting the **Custom
    value** option in the **Fill using** column and select the **three
    dots (...)** in the **Value** column.

    ![](./media/image50.png)

16. Select the **System** tab and search for **User**. Select
    the **User.Email** variable to get the email of the person using the
    agent

    ![](./media/image51.png)

17. Select **Save**

    ![](./media/image52.png)

### Task 6 - Define agent instructions

In this task, you will define the agent instructions for the Application
Intake agent.

1.  Move back in to the **Application Intake Agent** by selecting
    the **Agents** tab and selecting the **Application Intake Agent**.

    ![](./media/image53.png)

2.  In the **Instructions** field, paste the following clear guidance
    for your child agent.

    ```
    You are tasked with managing incoming Resumes, Candidate information, and creating Job Applications.  
    Only use tools if the step exactly matches the defined process. Otherwise, indicate you cannot help.  
    
    Process for Resume Upload via Chat  
     Upload Resume  
      - Trigger only if /System.Activity.Attachments contains exactly one new resume.  
      - If more than one file, instruct the user to upload one at a time and stop.  
      - Call /Upload Resume once. Never upload more than once for the same message.  
    
     Post-Upload  
      - Always output the [ResumeNumber] (R#####).
    ```

    ![](./media/image54.png)

3.  Where the instructions include a forward slash (/), select the text following the / and select the resolved name. Do this for,

    - System.Activity.Attachments (Variable)

    - Resume Upload (Tool)

    >[!Note] **Note:** If you click on the System.Acticvity.Attachements in the
    instructions, you will get the resolved name listed. You can select
    it. After selecting, if there is any part of the previously existing
    text available, please delete it.
    
    ![](./media/image55.png)
    
    ![](./media/image56.png)

4.  The instructions should now look like this.

    ![](./media/image57.png)

5.  Select **Save.**

    ![](./media/image58.png)

### Task 7 - Test your Application Intake Agent

Now let's verify that our agent is working correctly by calling our
child agent and following our instructions.

1.  **Toggle** the test panel open by selecting **Test**.

    ![](./media/image59.png)

2.  Select the Attachement icon, select the resume – AVERY EXAMPLE pdf from **C:\LabFiles\LabFiles**
    and click **Open**.

    ![](./media/image60.png)

3.  Give the message +++Process this resume+++ and hit **send**.

    ![](./media/image61.png)

4.  The agent should then give a message similar to **The resume for
    Avery Example has been successfully uploaded. The resume number is
    R1001.**

    ![](./media/image62.png)

5.  In the **Activity map**, you should see the **Application Intake
    Agent** handling the resume upload.

    ![](./media/image63.png)

6.  If the app is not open already, navigate to
    +++make.powerapps.com+++. Ensure the Dev One environment is selected
    in the top right Environment Picker. Select **Apps** → Hiring Hub →
    ellipsis(...) menu → **Play**  
    ![](./media/image64.png)

    **NOTE:** If the play button is greyed out it means you have not
published your solution. Select **Solutions** → **Publish all
customizations**.

7.  In the Power Apps – Hiring Hub app, navigate to **Resumes**, and
    check that the resume file is uploaded and the cover letter is set
    accordingly.

    ![](./media/image65.png)

## Exercise 2: Adding the Interview Prep connected agent

Now let's create our connected agent for interview preparation and add
it to your existing Hiring Agent.

### Task 1: Create the connected Interview Agent

1.  From the Copilot Studio, select the **Agents** tab in the left
    navigation and select the **drop down** next to **+ Create blank
    agent**, and select **Advanced create**.

    ![](./media/image66.png)

2.  Select the **Solution** as **Operative** and select **Confirm and
    create**.

    ![](./media/image67.png)

3.  Select **Edit** against the Details.

    ![](./media/image68.png)

4.  Provide the below details and select **Save**.

    - **Name**: +++Interview Agent+++

    - **Description**: +++Assists with the interview process.+++

    ![](./media/image69.png)

5.  Select **Edit** against **Instructions**, enter the below
    instruction and select **Save**.

    ```
    You are the Interview Agent. You help interviewers and hiring managers prepare for interviews. You never contact candidates. 
    Use Knowledge to help with interview preparation. 
    
    The only valid identifiers are:
      - ResumeNumber (ppa_resumenumber)→ format R#####
      - CandidateNumber (ppa_candidatenumber)→ format C#####
      - ApplicationNumber (ppa_applicationnumber)→ format A#####
      - JobRoleNumber (ppa_jobrolenumber)→ format J#####
    
    Examples you handle
      - Give me a summary of ...
      - Help me prepare to interview candidates for the Power Platform Developer role
      - Create interview assistance for the candidates for Power Platform Developer
      - Give targeted questions for Candidate Alex Johnson focusing on the criteria for the Job Application
      
    How to work:
        You are expected to ask clarification questions if required information for queries is not provided
        - If asked for interview help without providing a job role, ask for it
        - If asking for interview questions, ask for the candidate and job role if not provided.

    General behavior
    - Do not invent or guess facts
    - Be concise, professional, and evidence-based
    - Map strengths and risks to the highest-weight criteria
    - If data is missing (e.g., no resume), state what is missing and ask for clarification
    - Never address or message a candidate
    ```
    ![](./media/image70.png)

6.  Ensure that **Web Search** is **Disabled.**

    ![](./media/image71.png)

### Task 2: Configure data access and publish

In this task, you will configure the access to data and then publish the
agent.

1.  In the **Knowledge** section, select **+ Add knowledge.**

    ![](./media/image72.png)

2.  Select **Dataverse**  

    ![](./media/image73.png)

4.  In the **Search box**, type +++ppa\_+++. This is the prefix for the
    tables you imported previously in earlier lab.

5.  **Select** all 5 tables (Candidate, Evaluation Criteria, Job
    Application, Job Role, Resume). Select **Add to agent**

    ![](./media/image74.png)

5.  Select the **Settings** button in the upper right hand corner

    ![](./media/image75.png)

6.  Ensure that the following settings are configured.

    - **Let other agents connect to and use this one:** On

    - **Use general knowledge**: Off

    - **File uploads**: Off

    - **Content moderation level:** Medium

    ![](./media/image76.png)

    ![](./media/image77.png)

    ![](./media/image78.png)

7.  Select **Save** and select the **X** in the upper right hand corner
    to close out of the settings menu.

    ![](./media/image79.png)

8.  Select **Publish**.

    ![](./media/image80.png)

9.  Select **Publish** in the confirmation dialog and wait for the
    publishing to complete.

    ![](./media/image81.png)

### Task 3: Connect the Interview Prep Agent to your Hiring Agent

In this task, you will connect the Interview Prep agent to your Hiring
agent to achieve multi agent orchestration.

1.  Navigate back to your **Hiring Agent**. Select the **Agents** Tab
    and select **+Add an agent.**

    ![](./media/image82.png)

2.  Select the **Interview Agent**.

    ![](./media/image83.png)

    **NOTE**

    If the Interview Agent is greyed out and not selectable then that means it did not Publish. Go back to the Interview Agent and publish it first.

3.  Set the **Description** to be,

    ```
    Assists with the interview process and provides information about Resumes, Candidates, Job Roles, and Evaluation Criteria.
    Notice that the Pass conversation history to this agent is checked. This allows the parent agent to provide full context to the connected agent.
    Select Add and configure.
    ```

    ![](./media/image84.png)

4.  Ensure that you see both the **Application Intake Agent**, and
    the **Interview Agent**. Notice how one is a child and the other is
    a connected agent.

    ![](./media/image85.png)

    ![](./media/image86.png)

### Task 4: Test multi-agent collaboration

1.  **Toggle** the test panel open by selecting **Test**.

2.  **Upload** one of the test resumes (AVERY EXAMPLE or TAYLOR TESTPERSON pdf), and enter the following
    description which tell the parent agent what it can delegate to the
    connected agent:

    ```
    Upload this resume, then show me open job roles, each with a description of the evaluation criteria, then use this to match the resume to at least one suitable job role even if not a perfect match.
    ```
    
     ![](./media/image87.png)

3.  Notice how the Hiring Agent delegated the upload to the child agent,
    and then asked the Interview Agent to provide a summary and job role
    match using its knowledge.

     ![](./media/image88.png)

## Summary

You've successfully transformed your single Hiring Agent into a
sophisticated multi-agent orchestrated one with specialized
capabilities.

Here's what you've accomplished in this lab.

**Multi-agent architecture mastery**  
You now understand when to use child agents vs connected agents and how
to design systems that scale.

**Application Intake child agent**  
You've added a specialized child agent to your Hiring Agent that
processes resumes, extracts candidate data, and stores information in
Dataverse.

**Interview Prep connected agent**  
You've built a reusable connected agent for interview preparation and
successfully connected it to your Hiring Agent.

**Agent communication**  
You've seen how your main agent can coordinate with specialist agents,
share context, and orchestrate complex workflows.

**Foundation for autonomy**  
Your enhanced hiring system is now ready for the advanced features we'll
add in upcoming missions: autonomous triggers, content moderation, and
deep reasoning.





















