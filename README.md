# Data_Analysis
Data Analysis Course Group project
PREDICTION OF POTENTIAL DRUG-TARGET INTERACTIONS USING MACHINE LEARNING ALGORITHMS

Problem Identification
We chose to study the unknown interactions between existing drugs and the well-known proteins in human or animals.

Motivation
When we look at the drug database like https://www.drugbank.ca/, we found that for a specific drug, only the experimentally proved targets(proteins) are listed. However, it has been actively reported that except for the experimentally proved targets, the drug also interacts with other targets. That explains why drugs have side effects. We also saw some news that a drug was developed to cure a certain disease, but it turned out to be effective for curing other diseases. So, we are motivated to predict the unknown interactions using machine learning algorithms.

Kind of Data
We will use the data downloaded from Drugbank database. The data is saved in a csv file that lists the drug-target pairs which have been proved to be interacting with each other. The columns include drug ID, drug name, target ID and target name.

Data Source
We downloaded the data from Drugbank database: https://www.drugbank.ca/. In detail, we clicked the “Downloads” tag in the navigation bar of the homepage, then we went to “EXTERNAL LINKS”, and then we downloaded “Target Drug-UniProt Links” for DRUG GROUP “ALL”. The data is saved in the csv format.

Questions To Be Answered
1.	Predict the unknown interactions between drugs and targets. In other words, look for more drug-target pairs.
2.	Predict the interaction probability of each newly found drug-target pair. The researchers in the drug development area can use this predicted result as a direction to test the validity. This will contribute to the drug repurposing.
3.	Do the accuracy analysis of our predictions.

Preliminary Data Manipulation
1.	How you have imported your data
We used read_csv() function of pandas to import the data into a Jupyter Notebook.  

2.	A statistical summary of your data 
We used data.describe() to view summary of the data.
 
There are 20061 rows in total that indicates there are 20061 reported drug-target interactions. The total number of drugs is 7370. The total number of targets (identified by UniProt ID) is 4763. It is noted that the total number of Uniprot Name is 4255 which is less than the number of UniProt ID. We did some analysis to find out why and this is explained in the question #3 below.

3.	How you have cleaned your data (e.g., how you dealt with missing values)
We checked that there is no null value in the data.
 
By writing below code, we found that for some Uniprot Name, there are multiple UniProt ID matching it that leads to “more IDs than names”.

for name in set(data['UniProt Name']):
a=set(data.loc[data['UniProt Name'] == name]['UniProt ID'])
if len(a) > 1:
print(name,a)

 
We printed all the target names and the associated IDs once the number of IDs is greater than 1. For example, the target “Envelope glycoprotein gp160” has three IDs matching it: P05884, P35961, P12488. We searched these IDs in https://www.uniprot.org and found that they are linked to structurally the same protein but discovered from different organisms:
P05884: Simian immunodeficiency virus (isolate K6W) (SIV-mac) (Simian immunodeficiency virus rhesus monkey)
P35961: Human immunodeficiency virus type 1 group M subtype B (isolate YU-2) (HIV-1)
P12488: Human immunodeficiency virus type 1 group M subtype B (isolate BRVA) (HIV-1)
Since they are from different organisms, we treat them as different. Therefore, we use UniProt ID to identity targets in our study.

4.	An initial graphical summary of general aspects of our data.
data.groupby(['DrugBank ID']).count() is used to visualize how many interactions each drug is involved in.
 

data.groupby(['UniProt ID']).count() is used to visualize how many interactions each target is involved in.
 

