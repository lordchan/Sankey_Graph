# Sankey_Graph
## Goal: A step-by-Step guide to create a Sankey Diagram in Python.
Source: I have taken inspiration from this article in Medium. Big thanks to the author. https://medium.com/@cbkwgl/sankey-diagrams-in-python-fc9673465ccb
### Note: I would highly recommend you to go through the article before starting this project.
Dataset can be downloaded here [https://github.com/lordchan/Sankey_Graph/blob/main/sankey_table]
## Understanding the data:
The Data is taken from a HealthTech startup that provides pregnancy testing kits. And the result of the test is used to determine whether the customer remains or churns.
We have the 3 columns - user_id, cycle number and stage.
user_id is just the unique id of a particular user, stage is what determines whether a customer churn or not based on which cycle she is in. In stage column we have - Churn, No Churn and potential customer. We need to make a Sankey diagram showing the number of people churning/non churning for each cycle number. So it should hopefully be such that cycle number should progress horizontally and the number of people in each stage should be distributed vertically for each cycle. Sankey diagram is used to show the number of people moving in and out of each stage after every cycle.
### Step 1: Install the packages - Pandas and holeviews
### Step 2: Load the dataset.
### Step 3: Data Manipulation: 
We want the data in 3 columns - source, target, and value. And the data should strictly be in this order and column name. Otherwise the holeview package will throw an error.
The source column contains the starting edge of sankey and target column should be the end of the edge. Its as if like water flow. The source should be inlet and target is the outlet. The flow is the number of people transition from previous cycle to the current one. Thicker the flow more the count of people. 
Now the data format we have and the data format holeview takes is completely different. How do we arrive at this type of forma?
After experimenting I found out a way to achieve this. And there are many ways to do it, I came up with simple and elegant way of doing it.
So basically we need the count of people transitioning from 1 source to another target. Every cycle number has multiple stages. So we have different combination of cycle number and stage name. If we are able to find out for each unique pair of these cycle/stage combination and find out the number of people moving onto the next cycle then we have essentially solved the problem. This is achieved by self joining the table on user_id based on the condition - the cycle number in 2nd table should be one more than cycle number in first table. This way we will only be mapping rows where users have transitioned to the next cycle.
In order to get a count of the number of people transistioning we just have to group by all these unique pairs of cycle/stage. 
For the sake of simplicity and easier representation we concatenate the cycle and stage into one column. Now we will call the first combination as 'source' and second combination as 'target' and the count as 'value'. This is the key block in our project because making the Sankey diagram is easy, preparing the data takes a lot of time.
### Step 4: Making the diagram:
Now we have the final table ready, we just have to run the holeview.Sankey function and pass in our data. 
