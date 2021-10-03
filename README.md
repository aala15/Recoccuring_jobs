# reoccurring-jobs_detection

Please download and unzip the data folder in the root directory of the project.
Link to data folder: https://tubcloud.tu-berlin.de/s/YQffxrJXjmCM8P8



| Name                          | Description                                                | Generated in              | Used in                                                |
|-------------------------------|------------------------------------------------------------|---------------------------|--------------------------------------------------------|
| batch_task.csv                | main data                                                  | _                         | Preprocessing,Vector                                   |
| DAGs_igraph_dict.pickle       | dictionary of igraph dags object, the islands are deleted. | preprocessing             | Vector                                                 |
| ruleBased_DAGs.csv            | dataframe  dags with new detected similar jobs             | preprocessing             | Vector, rule_based, clustering ***                        |
| ruleBased_DAGs_dict.npy       | dictionary of isomorphic items                             | preprocessing             | Preprocessing, Vector,                                 |
| preprocessed_jobs.csv         | preprocessed of batch                                      | preprocessing             | Vector                                                 |
| vectorDF.csv                  | vector of 22 attributes                                    | Vector                 | Vector, Vector_evaluation, GNN, Clustering, Rule-based |
| PyG_vector_allDAGs.pickle     | dictionary of pyg with 22 features                         | Vector                 | Vector                                                 |
| PyGO_vector_NNprepared.pickle | nn prepared dictionary                                     | Vector, Neural_network | GNN                                                    |
| embedDf ***                    | embeddings, there are alot of them                         | Neural_network            | Clustering                                             |
| aggFeatures.csv               | aggregation of graph level attributes                      | Aggregated_feature        | Clustering, Rule-based                                 |

** The different embeddings are generated in csv files started with "model". The methodology of GLP as well as size of embeddings are described in the name of the fine.

*** clustering scripts are K-means, DBSCAN, OPTICS. All the embeddings are used in OPTICS. the AVG embedding size of 64 is used in K-means and DBSCAN


**Preprocessing**

Input: batch_task

Output: a dictionary of igraph objects, dictionary of isomorphism graphs, rule-based classified jobs


the input is batch-task and the output is dictionaries needed for further analysis
The rule-based classification is also determined here. 
No prediction of the runtime is applied here


till first checkpoint, further data manipulating runtime 1-2 hours
third checkpoint: highly time-consuming (more than 2 days), the igraph objects extraction, rule-based classification
__________________________________________________________

**Vector**

Input: batch-task, dictionary of igraph objects

Output: a dictionary of PyG objects along with the target variable, dataframe of vector for the jobs

Target variable definition and preparing the data for the GNN, the Histogram of DAGs density, runtime, #nodes
This script runtime ca 24 hours, 

#First checkpoint: extracting the y vector and saving in a dictionary (16 hours)

#Second checkpoint: Outliers elimination, generating PyG objects (lesser than 4 hours), generating a dataframe of Y vector

__________________________________________________________

**Feature selection**

input: vector_df

output: best five features of each feature selection method


detection of target variables
runtime: lesser than one hour
the feature selections are: linear regression, RFE, PCA, ExtraTreesClassifier
__________________________________________________________

**Aggregation_feature**

Input: batch_task

Output: aggFeatures.csv

Runtime: A couple of minutes
Calculate the aggregation of features: mean, sum, max, var
__________________________________________________________

**Rule-Based Prediction**

Input: vectorDF.csv, ruleBased_DAGs.csv, aggFeatures.csv

Output: prediction of runtimes using classes determined by rule-based classification

Runtime: lesser than 30 minutes
Prediction with different scenarios, calculation of statistics over identified groups
Applying prediction using multiple aggregated features namely: var, max, sum, mean, no, all

__________________________________________________________

**GNN**

Input: a dictionary of prepared PyG objects (without outlier) and vecttor dataframe

Output: DAGs embedding 

Runtime: ca. 6-12 hours, depending on settings of neural network and early stop. 
This script is executed with multiple settings, the changing parameters are the loss function, GPL, and embedding size. One can reproduce the results by using the same settings described in the name of the embeddings' dataset.
__________________________________________________________

**OPTICS**

Input: ruleBased_DAGs, aggFeatures, vectorDF, multiple types of embeddings

Output: Statistical results over performance of different embeddings



runtime: ca 2-3 hours (In case of reading already generated OPTICS couple of minitues)
Evaluation of different embeddings

Notes: 

(1)The OPTICS models are already stored. The methodology of generating the models are also available. One can uncomment the related part to reproduce the model

(2)The embeddings are as follows: 
AVG:16, 32, 64, 60, 128
SUM:32, 60
MAX: 32, 60
Combination of all: 96 (32+32+32)
__________________________________________________________

**K-Means**

input: ruleBased_DAGs, aggFeatures, vectorDF, embed64

output: statistics over performance of k-means



runtime: a couple of hours (depends on selection of subset of data)
Generating K-means on multiple subsets of the dataset.
Notes: 
(1)the masks are already written. To reproduce the results, uncomment the mask and run the rest of the code.

(2)the elbow method for the first musk is performed. Due to intensive runtime, it was not feasible to select appropriate k for the whole dataset through the elbow method. 
__________________________________________________________

**DBSCAN**

input: ruleBased_DAGs, aggFeatures, vectorDF and the AVG embeddings 

output: runtime prediction

runtime: ca. 1-2 hours (parameter selection part is the most time-consuming part)
This script analyses the data and search for optimal value of radius and MnPts,
then apply the clustering using these parameters and apply the prediction. The final part, make a comparison between predictions of baseline with DBSCAN











