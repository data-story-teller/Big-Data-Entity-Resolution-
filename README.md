# Big-Data-Entity-Resolution-Project

The project work includes the implementation of the algorithms (Token Blocking, Attribute Clustering Blocking and Meta Blocking) that realize steps of the entity resolution process. To implement these algorithms python used as a programming language and jupyter notebook used as a tool. Programs are run using jupyter notebook. 

 To work on this project, we have created two clean-clean Entity Collections. Each entity collection contains 150 student's demographic information. There are 30-35% matching entities (but no duplicates within themselves) are present in an entity collection. First entity collection ‘Entity_Collection_1.csv’ has six columns (id, name, phone, gender, university, job_title) and the Second entity collection ‘Entity_Collection_2.csv’ also contains six similar columns (id_num, full_name, phone_num, sex, school, designation). Where id and id_num contains the unique identity of each student, name and full_name contains the name of each student, phone and phone_num contains the contact of the correcponding student, gender and sex contains the sex of the student, university and school contains the information of the educational institution of each student, job_title and designation contains the current job tiltle of the student. Before working on those entity collections, some preprocessing has been performed over the data. For example, in the ‘phone’ and ‘phone_num’ attribute we removed unwanted hyphen as well as the country code then make each number as 10 digits. 

### Implementation of Token Blocking:

 The main idea for the token blocking is to place entities in a block such a way to reduce the number of comparisons among entities. To do so, at first, we take two clean sets ‘Entity_Collection_1.csv’, ‘Entity_Collection_2.csv’ of entity descriptions as Clean-Clean Entity Resolution. Then, each distinct token ‘ti’ of each value of each description in (Entity_Collection_1 U Entity_Collection_2) creates a separate block ‘bi’ that contains all entities having ‘ti’ in the values of their profile regardless of the associated attribute names.

### Problems of Token Blocking:
1. The same pair of descriptions is contained in many blocks 
2. Many dissimilar pairs are put in the same block

### Implementation of Attribute Clustering Blocking:

Token blocking totally ignores the valuable information of attribute names. To improve this, attribute clustering is introduced. The main idea of cluster blocking is as follows.

1. For each attribute of the Entity_Collection_1, Find the most similar attribute of Entity_Collection_2. 
2. For each attribute of the Entity_Collection_2, Find the most similar attribute of Entity_Collection_1.
3. Compute the transitive closure of the generated pairs of attributes to generated attribute clusters. 
4. Finally, Perform Token Blocking on the clusters.

In step 1 and step 2, Jaccard Similarity measures have been used. 

### Implementation of Meta-Blocking method:

The steps for the meta-blocking process are to iterate over the list of blocks generated using the token blocking approach. Afterward, from each block it creates a list of unique entities as well as the list of entity pairs. Here, each unique entity will be denoted as node and each entity pairs will. be denoted as edges. We have calculated the weight for all the edges using two schemes:
       1. Common Block Scheme:
       2. Jaccard Scheme: 

Afterwards, we have pruned the model or model based on weighted Edge pruning and Cardinality Node Pruning. Finally, we have generated blocks from the remaining edges of the pruned model by transitive closure.

# Evaluation Methods:
To evaluate the performace of the blocking methods three performace metric has been used.
- Reduction ratio (RR): Reduction Ratio is defined as: RR = 1 - S/N

  where S is the number of record pairs produced by a blocking method for comparison and N is the number of possible record pairs in the entire data sets (assuming we link two data sets with n records each N = n×n). RR is the relative reduction in the number of record pairs to be compared.

- Pairs completeness (PC): Pairs completeness defined as PC = SM⁄NM, 

  where SM is the number of true match record pairs in the set of record pairs produced for comparison by the blocking method and NM is the total number of true match record pairs in the entire data.
  
- Pair quality (PQ): Pair quality defined as PQ = SM ⁄ S, where SM is the number of true match record pairs in the set of record pairs produced for comparison by the blocking method and where S is the number of record pairs produced by a blocking method for comparison.

# Results
In this project two blocking methods (token blocking and attribute clustering blocking) is implemented for entity resolution. Also, this project has discussed how the blocking methods can be optimized via meta-blocking schemes, which restructure the block collection to improve efficiency while trying to keep effectiveness intact. 
After analyzing all the results it is evident that meta-blocking improve efficiency and  not reduce effectiveness at all. This is due to the simplicity of the  entity collection, as they are not very heterogenous. With other datasets it could be possible that the pruning steps in the various meta-blocking schemes lose some legitimate matches.

# Reference:
1. Papadakis, Ekaterini Ioannou, Themis Palpanas, Claudia Niederee, and Wolfgang Nejdl, ‘A Blocking Framework for Entity Resolution in Highly Heterogeneous Information Spaces George’ [http://l3s.de/~papadakis/erData/BlockingFrameworkCameraReady.pdf]
       
2. George Papadakis, Georgia Koutrika, Themis Palpanas, and Wolfgang Nejdl, ‘Meta-Blocking: Taking Entity Resolution to the Next Level’ [https://helios2.mi.parisdescartes.fr/~themisp/publications/tkde13-metablocking.pdf]


