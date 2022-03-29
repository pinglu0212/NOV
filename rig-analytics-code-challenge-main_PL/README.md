# Rig Analytics Team Code Challenges

This is the Rig Analytics code challenge at NOV.

## Instructions to run the code
* We're going to use Python 3.7 with Anaconda Jupyter Notebook. 

* Let's start by downloading the latest version of Anaconda: https://www.anaconda.com/distribution/#download-section
 
* Once you install, you can simply start a blank notebook by typing in the Terminal Window with:
>> jupyter notebook
 
* Then you can open the file named "NP_Optimization.ipynb".

* You can run each cell by clicking on the "Run" button in the header.
 
* When you are done and close out jupyter, you can deactivate the current environment
>> source deactivate

* You can find more info on conda here: http://conda.pydata.org/docs/using/pkgs.html


## Description of the problem
We have 5000 customer appointments and 150 tellers. We want to assign all the 5000 appointments to these 150 tellers. We want to create such a balanced schedule that the maximum duration of the tellers' total service time is minimized. In other words, we want all the 5000 jobs done as quickly as possible, but in a very balanced way such that the duration of all appointments for the teller with the longest total duration is minimized. (We want to keep in mind that when a customer's service type matches the specialty type of the assigned teller, the multiplier of the teller will be multiplied against the customer's appointment duration to reduce the appointment time. Also note that we can't consider only assigning matched customer appointment to the matched SpecialtyType tellers. Because some type of appointments are very few and short. We might end up with some specialty tellers not having enough to do while others being too busy, if we only consider let the professionals do their own specialty work.)

## Steps of the solution and reasoning
It is illustrated in Workflow 1.jpeg and Workflow 2.jpeg how the matching game would play out in the iterations below.

### 1. Data Exploration - Getting Basic Statistics of the Dataset
Number of Customer Appointments: 5000; Type 1; Type 2; Type 3; Type 4;
Number of Tellers: 149; Specialty 1 #: 29; Specialty 2 #: 50; Specialty 3 #: 50; Specialty 0 #: 20.

### 2. Estimate the duration needed for each teller category and pick the SpecialtyType with minimum time.
Given Customer appointment and Teller information, estimate how much time each teller will spend on average in each category if each teller will only service customer appointments that are a match of their own specialty. Here we begin by categorizing Teller Specialty 0 as "unmatched", and Customer Type 4 as "unmatched" as well. The multipliers for "unmatched" tellers are always 1. Tellers of SpecialtyType "unmatched" will service "unmatched" type of customer appointments, and tellers of SpecialtyType "1" will service customer appointments type of "1" etc. This function is implemented in function def estimate_time_of_a_type(customer_info, teller_info). We return the SpecialtyType with the minimum duration among all the SpecialtyTypes, and get that minimum duration as well.

Reasoning for this estimation:
We want to first assign the amount of appointment time each SpecialtyType of tellers will get evenly when they are doing their specialty work including the "unmatched". The estimated time each teller will spend on average in each category is calculated as = duration of customer appointments in each category * average multiplier for the tellers/ number of tellers in this category. This estimated time will be stored in teller_info as "need_time". Then we pick the SpecialtyType with the minimum duration among all the SpecialtyTypes, and get that minimum duration as well.

For example, for SpecialtyType 1, we have 36775 mins of customer appointment in that category to begin with, the average multiplier for SpecialtyType 1 tellers is 19.25/29=0.6638. So on average, each 
SpecialtyType 1 teller will have 36775*0.6638/29= 841 mins on their matching customer appointment list. Similarly, SpecialtyType 2 teller will have 441 mins on their matching customer appointment list.
SpecialtyType 3 teller will have 128 mins on their matching customer appointment list.
"unmatched" teller will have 870 mins on their "unmatched" customer appointment list.
So SpecialtyType 3 have the least amount of estimated appointment time (128 mins) on their list, we can start assigning equal to or slightly larger than 128 mins of matched appointments for each category.

### 3. Match Customers with Tellers:
#### 3.1 Match Tellers and Customer Appointments in their Corresponding Categories
Since we picked the SpecialtyType of 3 with 128 mins in step 2, we want to assign all customer appointments with Type 3 to tellers with SpecialtyType3. We also want to assign other SpecialtyType tellers with equal to or slightly larger than 128 mins of actual customer assignments. We always assign the longest appointment in this category to the teller with the minimum time used in the same category. We need to maintain a MaxHeap for the remaining customer appointments and a MinHeap for the tellers' time used. This is implemented in the function def match(type_, customer_info, teller_info, customer_maxheaps, teller_minheaps, customer_done, time_limit).

#### 3.2 Update the Information
If we just finished matching all customers in a category that's not in the "unmatched" category with the corresponding tellers, then we move all the tellers in this category to the "unmatched" category.
If we just finished matching all customers in a category that's in the "unmatched" category with the corresponding tellers, then we do not move the tellers in this "unmatched" category. We keep the tellers of "unmatched" in its category in place and prepare them for new assignments. We move some of other types of customers to the "unmatched" category of customers as well. This is implemented in the def update_info (ustomer_info, teller_info, customer_maxheaps, teller_minheaps) function.

### 4. Repeat Step 2 to 3 until All the Appointments are Assigned
If we have any customers left in the customer_info dict, we are not done with the assignment yet. Otherwise, we terminate the iteration and generate the results.

### 5. Generate the Results with Graphs and Scheduler Files.
For the finalized schedule, the maximum duration of all tellers is 498.75 min.
The customers_match.csv includes all customers' appointment information.
The tellers_match.csv includes all tellers' appointment information.

## Assumptions Made:
When we just finished matching all customers in a category that's in the "unmatched" category with the corresponding tellers, then we do not move the tellers in this "unmatched" category. We keep the tellers of "unmatched" in its category in place and prepare them for new assignments. We move some of other types of customers to the "unmatched" category of customers as well. We get the type of customers with the most duration left to assign, and the minimum duration from other types left. If we have more than two categories of customers left, one type of customer's duration is significantly (approximately at least twice the duration of the other) longer than the other, then we move some customers from the larger duration to the "unmatched" category, and the duration is equal to the smallest duration in the categories left. If one type of customer's duration is only slightly (an approximation) longer than the other, then we move some (estimated from the duration difference) customers from the larger duration to the "unmatched" category.

## Future Work:
The Step 4, Update the Information Step has the above assumptions made, I'd like to further explore a better approximation to make the scheduler more optimized.
   





