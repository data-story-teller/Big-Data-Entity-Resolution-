# Big-Data-Entity-Resolution-
Description of Data:	
To work on this project, we have created two clean-clean Entity Collections. Each entity collection contains 150 students’ demographic information. There are 30-35% matching entities (but no duplicates) are present in an entity collection. 
First entity collection ‘Entity_Collection_1.csv’ has six columns: 
1. ‘id’: This column contains the unique identity of each student's.
2. ‘name’: The name of each student. 
3. ‘phone’: Their contacts’ number of the corresponding student. 
4. ‘gender’: The sex of the students. 
5. ‘university’: The educational institute of the student
6. ‘job_title’: The current job title of the student.
Second entity collection ‘Entity_Collection_2.csv’ also contains six similar columns:
1. ‘id_num’: This column contains the unique identity of each student's.
2. ‘full_name’: The name of each students.
3. ‘phone_num’: Their contacts’ number of the corresponding student.
4. ‘sex’: The sex of the students.
5. ‘school’: The educational institute of the student
6. ‘designation’: The current job title of the student. 
Before working on those entity collections, some preprocessing has been performed over the data. For example, in the ‘phone’ and ‘phone_num’ attribute we removed unwanted hyphen as well as the country code then make each number as 10 digits. 

Implementation of Token Blocking:
The main idea for the token blocking is to place entities in a block such a way to reduce the number of comparisons among entities. To do so, at first, we take two clean sets ‘Entity_Collection_1.csv’, ‘Entity_Collection_2.csv’ of entity descriptions as Clean-Clean Entity Resolution. Then, each distinct token ‘ti’ of each value of each description in (Entity_Collection_1 ? Entity_Collection_2) creates a separate block ‘bi’ that contains all entities having ‘ti’ in the values of their profile — regardless of the associated attribute names.
Example: 
Two entities from two entities collection.
ide1_001id_nume2_001nameHenry Kellyfull_nameDoris Tuttonphone3211734531phone_num1367452581genderMalesexFemaleuniversityUniversity of VirginiaschoolUniversity of California, Santa Barbara (UCSB)job_titleHR SpecialistdesignationAccountant
Some generated blocks with entities mapped:
HenryKelly3211734531VirginiaHRe1_001 e1_136 e2_136e1_001'e1_001''e1_001', 'e1_074', 'e1_123', 'e2_008', 'e2_034', 'e2_043''e1_001','e1_004','e1_012', 'e1_135','e1_138','e1_146',
'e1_150','e2_087','e2_090',
'e2_112','e2_126','e2_135',
'e2_138','e2_146','e2_150'
Problems:
* The same pair of descriptions is contained in many blocks 
* Many dissimilar pairs are put in the same block



Implementation of Attribute Clustering Blocking:
Token blocking totally ignores the valuable information of attribute names. To improve this, attribute clustering is introduced. The main idea of cluster blocking is as follows.
1. For each attribute of the Entity_Collection_1, Find the most similar attribute of Entity_Collection_2. 
2. For each attribute of the Entity_Collection_2, Find the most similar attribute of Entity_Collection_1.
3. Compute the transitive closure of the generated pairs of attributes to generated attribute clusters. 
       Pairs:  
        [('name', 'full_name'), ('full_name', 'name')],                    	       	        	 [('phone', 'phone_num'), ('phone_num', 'phone')],
       [('gender', 'sex'), ('sex', 'gender')],
       [('university', 'school'), ('school', 'university')],
       [('job_title', 'designation'), ('designation', 'job_title')]]
      Clusters:
       ['name', 'full_name'], ['phone', 'phone_num'], ['gender', 'sex']        	['university', 'school'], ['job_title', 'designation']
4. Finally, Perform Token Blocking on the clusters.
In step 1 and step 2, Jaccard Similarity measures have been used. Suppose two attributes        A= name and B= school, then Jaccard Similarity can be described as follows:
 




Implementation of Meta-Blocking method:
The steps for the meta-blocking process are to iterate over the list of blocks generated using the token blocking approach. Afterward, from each block it creates a list of unique entities as well as the list of entity pairs. Here, each unique entity will be denoted as node and each entity pairs will. be denoted as edges.  
We have calculated the weight for all the edges using two schemes:
1. Common Block Scheme:
2. Jaccard Scheme: 
Afterwards, we have pruned the model or model based on weighted Edge pruning and Cardinality Node Pruning. Finally, we have generated blocks from the remaining edges of the pruned model by transitive closure.

Common Blocks Scheme: 
After identifying the unique entities as nodes and unique entity pairs as edges of a model, we calculate the weights for the edges based on common blocks scheme. So, we define the weight of each edge (each entity pair) as the total number of blocks they appear.
       e_(i,j).weight?=_|B_(i,j) | 
Here, |B_(i,j) | is the set of blocks shared by the entities p_i and p_j.
For example, for an entity pair (e1_003, e2_143) the weight is assigned as 3, as it appears in three distinct blocks.
For example, {('e2_001', 'e1_001'): 2} with CBS weight value 2 means node 'e2_001' & 'e1_001' has been co-occurrence in 2 different blocks from both entity collections. 

Jaccard Scheme: 
After identifying the unique entities as nodes and unique entity pairs as edges of a model, we calculate the weights for the edges based on Jaccard scheme. So, we define weight of each edge (each entity pair) with the value calculated as the ratio of total number of blocks with both entities are present together and sum of total number of blocks with only each entity present.
       e_(i,j).weight=|B_(i,j) |÷(|B_i |+|B_j |-|B_(i,j) |)
Here, |B_(i,j) | is the set of blocks shared by both entities p_i and p_j . |B_i | is the set of blocks shared by only p_i. |B_j | is the set of blocks shared by only p_j.
For example, {('e2_001', 'e1_001'): 0.1053} means this pair of edges has Jaccard similarity score 0.1053.

Weight Edge Pruning: 
Once we build a model with all the nodes, edges and edge weights, we apply weighted edge pruning on the model which is we only keep the edges with weights greater than the average of all the weights in the model. From the pruned model we extract blocks that have all the connected nodes.
For the example entities given in network model, the edge weights are calculated by Common Blocks Scheme and model pruning is based on Weighted Edge Pruning. Finally, the redundant comparisons generated from token blocking approach are avoided using meta blocking approach.

Cardinality Node Pruning:
Once we built a model with all the nodes, edges and edge weights, we apply cardinality node pruning on model which is for each node in model we make a list of all the edges connected to the node and sort them with respect to edge weights in descending order. We calculate the k- threshold for each node-model (model with the node and its connected neighbours) as
      K = BCin-1
Where BCin is the blocking cardinality of node-model calculated as the ratio of total number of blocks with all the pairs of node-model and total number of entities in node-model.
      BCin = ? Be_i,e_j/ ? e_i
Here,  ?Be_i,e_j is the total number of blocks with all the pairs of node-graph (?Bei,ej) and ?e_i is the total number of entities in node-graph. 
We then keep the top k  edges of each node in the model and from the pruned model we extract blocks which have all the connected nodes.
Observation of both pruning methods:
We have used both Common. Block Scheme and Jaccard Scheme for both pruning methods. The initial number of blocks that we have created after token blocking is 880. Afterward, we have normalized the blocks by removing blocks which have only one element. So, we calculated the weight of each edge from the 388 blocks that we have got after reducing the single elements blocks. 
Num of blocks after Weight Edge Pruning w/ Common Block SchemeNum of blocks after Weight Edge Pruning w/ Jaccard SchemeNum of blocks after Cardinality Node Pruning w/ Common Block SchemeNum of blocks after Cardinality Node Pruning w/ Jaccard Schemenumbers273273313325percentage of reduction29.6%29.6%19.3%16.2%
From the above discussion we can see that overall Weight Edge Pruning has higher reduction ratio than Cardinality Node Pruning. 
Reference:
Papadakis, Ekaterini Ioannou, Themis Palpanas, Claudia Niederee, and Wolfgang Nejdl, ‘A Blocking Framework for Entity Resolution in Highly Heterogeneous Information Spaces George’ [http://l3s.de/~papadakis/erData/BlockingFrameworkCameraReady.pdf]
George Papadakis, Georgia Koutrika, Themis Palpanas, and Wolfgang Nejdl, ‘Meta-Blocking: Taking Entity Resolution to the Next Level’ [https://helios2.mi.parisdescartes.fr/~themisp/publications/tkde13-metablocking.pdf]


