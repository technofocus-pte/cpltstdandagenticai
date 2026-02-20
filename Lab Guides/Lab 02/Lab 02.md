# Lab 2: Creating an intelligent hiring agent for talent acquisition

In this lab, you'll establish the foundation for your hiring automation
system. You'll begin by importing a pre-configured solution that
contains all the necessary Dataverse tables and data structure for
managing candidates, job positions, and hiring workflows. Next, you'll
populate these tables with sample data that will support your learning
throughout this module and provide realistic scenarios for testing.
Finally, you'll create the Hiring Agent in Copilot Studio, setting up
the basic conversational interface that will serve as the cornerstone
for all the other features you'll add in future missions.

## Exercise 1: Import solution

In this exercise, you will import a pre existing Solution.

1.  Go to Copilot Studio at +++https://copilotstudio.microsoft.com+++

2.  Select the **...** in the left navigation and select **Solutions.**

    ![](./media/image1.png)

3.  Select **Import solution**. Click on **Browse** and select the
    **zip** file starting with **Operative** form **C:\LabFiles\LabFiles** and
    select **Open**.

    ![](./media/image2.png)
   
    ![](./media/image3.png)

    ![](./media/image4.png)

4.  Once selected, select **Next** and then select **Import**.

   ![](./media/image5.png)
   
   ![](./media/image6.png)

5.  This will take some time of around 3 to 5 minutes. On success, you
    will see a green notification bar with the following message when
    it's done: "Solution "Operative" imported successfully."

   ![](./media/image7.png)

6.  Once you see the "imported successfully" message, take a look at
    what you imported by selecting the display name of the solution
    (**Operative**) in the solutions list.

   ![](./media/image8.png)

7.  Review the solution and ensure that the following components are
    imported.

   ![](./media/image9.png)

8.  Select the **Publish all customizations** button at the top of the page.

   ![](./media/image10.png)

## Exercise 2 - Import sample data

In this exercise, you will add sample data to some of the tables that
you imported in the previous exercise.

1.  From the solution that you imported in the last exercise, select
    the **Hiring Hub** Model-Driven App by selecting the checkmark in
    front of the row and select the **Play** button at the top.

   ![](./media/image11.png)

2.  Select **Job Roles** in the left navigation. Select
    the **More** icon (three dots below each other) in the command bar
    and then select the **right arrow** next to **Import from Excel.**

   ![](./media/image12.png)

3.  Select **Import from CSV**.

   ![](./media/image13.png)

4.  Select the **Choose File** button, select the **job-roles.csv** file
    from **C:\LabFiles\LabFiles** and then select **Open**.

   ![](./media/image14.png)

5.  Select **Next.** Leave the next step as is and select **Review
    Mapping**

   ![](./media/image15.png)
   
   ![](./media/image16.png)

6.  Make sure the mapping is correct and select **Finish Import**.

   ![](./media/image17.png)

7.  Select **Done**. This can take a little while, but you can hit
    the **Refresh** button to see if the import has succeeded.

   ![](./media/image18.png)
   
   ![](./media/image19.png)

8.  Now, you will import the **Evaluation Criteria sample data**

9.  Select **Evaluation Criteria** in the left navigation.

10. Select **Import from CSV** like you did earlier. Select the **Choose
    File** button, select the **evaluation-criteria.csv** from
    **C:\LabFiles\LabFiles**.

   ![](./media/image20.png)

11. Select **Next**. Leave the next step as is and select **Review Mapping**

   ![](./media/image21.png)

   ![](./media/image22.png)

   >[!Alert] **Important:** If you do not see the mapping to be proper in the page, then select **Back** and select the **Data Delimiter** to be **None** and select **Review Mapping**
   >
   >![](./media/image31.png)

12. Now we have to do a bit more work for the mapping. Select the
    **magnifying glass icon** next to the **Job Role** field.

   ![](./media/image23.png)

13. Make sure **Job Title** is selected here, and if not - add it and
    select **OK**.

   ![](./media/image24.png)

14. Make sure the rest of the mapping is correct too and select **Finish
    Import** and then select **Done**.

   ![](./media/image25.png)

15. This can take a little while, but you can hit the **Refresh** button
    to see if the import has succeeded.

   ![](./media/image26.png)

## Exercise 3 - Create the hiring agent

Now you are done with the setup of the prerequisites, it's time for the
actual work! Let's add our Hiring Agent first!

1.  From the Copilot Studio, select **Agents** from the left pane. Select
    the drop down next to the **+ Create blank agent** and select **Advanced create**.

    ![](./media/image27.png)

2.  In the Agent settings, select the Solution as **Operative** and then
    select **Confirm and create**.

    ![](./media/image28.png)

3.  Select **Edit** against the Details of the created agent.

    ![](./media/image29.png)

4.  Enter the name as +++**Hiring Agent**+++ and Description as
    +++**Central orchestrator for all hiring activities**+++ and select
    **Save**.

    ![](./media/image30.png)

## Summary

In this lab, you now have complete the following.

- **Scenario Understanding**: Comprehensive knowledge of hiring
  automation challenges and the solution you will be building.

- **Solution Deployment**: Successfully imported and configured the
  building blocks of the hiring management system.

- **Agent Creation**: Built an hiring agent that is the start of the
  scenario you're going to build as an Agent Academy Operative








