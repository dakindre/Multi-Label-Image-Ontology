# Multilabel Image Classification and Graphical Database Ontology

## Understood Objective
Given a set of unlabeled images, create an ontological representation of image classification hierarchy

## Table of Contents 
1.	Introduction
2.	Technologies Used
2.1.	Final Production
2.2.	Development
3.	Procedures and Justifications
3.1.	Image Detection
3.1.1.	 Limitations of Zalando’s and Other Open Source Computer Vision Models
3.1.2.	 Rekognition Model
3.1.2.1.	Misclassification of objects
3.1.2.2.	Understanding the Classification Logic
3.2.	Inferring the Classification Structure from Logic
3.2.1.	 Elimination of Noise
3.2.2.	Creating a classification structure template
3.2.2.1.	Super-domain and Sub-domain
3.2.3.	Programmatic Interpretation of Classification
3.3.	Ontological Interpretation of Data
3.3.1.	 Understand the Relationship of RDF and LPG
3.3.2.	 Importing Data into LPG
3.3.3.	 Graphical View of Data
3.3.4.	 Programmatic View of Data
3.3.4.1.	Querying the Database
4.	Reproduce 
4.1.	File Explanation
4.2.	Procedure

## Introduction
Enter something here

## Technologies Used
### Final Production
Anaconda Python – Jupyter Notebook/AWS CLI
AWS Rekognition – Image Classification
Neo4j – Graph Database

### Development
Protégé – Ontology Editor
Jena – Ontology Editor
Keras/Tensorflow: Dataflow programming software


## Procedures and Justifications
### Image Detection
In order to classify the images into an ontology a computer vision model must first be used to detect objects within each image. The model would need to be trained on a diverse image set such as “ImageSet” and provide multilabel classification as an output. My approach was to use an image detection model in which all objects with a confidence match of greater than 50% would be included. This would effectively identify items within the image that have logical relationships with one another.

#### Limitations of Zalando’s and Other Open Source Computer Vision Models
I initially thought to use a dataset created for identifying e-commerce. Ideally the dataset would have a diverse sampling of fashion items. For this I used Zalando’s FMNIST dataset which provided a set of 60,000 images and 10 category labels. This model had poor performance when used against the Resonance Image Set and was too simplistic.

#### Rekognition Model
Amazons Rekognition model provided the comprehensiveness that was needed to identify all objects within the images with a certain degree of accuracy. The software was included under free tier and was assumed to qualify as open source. Using the boto3 API I created a label dataframe locally. The script can be seen [here]

##### Misclassification of objects
1.	Graphics or patterns that contained images such as deer
2.	Sketched Images
3.	Solid colored objects that didn’t show texture

##### Understanding the Classification Logic
The model detected both primary domains and subdomains e.g. skirt and miniskirt. However the model did not create any relationship links between items detected. In the example shown below we see 3 objects. The model will not tell you which descriptor belongs to which object. That logic needs to be done in the next step
[Image Here rekognition_sample1]

### Inferring the Classification Structure 
Initially I wanted to use an NLP model to detect relationships in order to create a class ordering, but could not find one that fit. My next idea was to use a prepopulated ontology that could infer the structure from the labels. This was the original objective but the OWL ontologies I found were not comprehensive enough to achieve this. 
In order to create logical order of the image labels I displayed all detected labels grouped by count [grouped meta]. The grouping provided a general understanding of the hierarchical structure used by Rekognition e.g. Human[711]->Female[446]->Girl[1]

#### Elimination of Noise
In order to achieve the above it was obvious that noise labels must be removed to make this a manageable task. Occurrences of 1 that had no relevance to the assumed fashion domain were removed. Many of these were background objects e.g. zebra stripes on asphalt. Additionally, redundant labels such as “Human” and “Person” with the same occurrence counts were reduce to one label. This logic reduced the result set by nearly half. 

#### Creating a classification structure template
The final classification model can be seen [here].


##### Super-domain and Sub-domain

The reordering and grouping resulted in 12 super-domains and up to 7 sub-domains.
In the super-domain Apparel [1466] for example, logical connections were made between labels e.g. Skirt->Miniskirt. A miniskirt must belong to its superclass of Skirt.  Below is an example of what would happen if this were not the case
[show example here]

#### Programmatic Interpretation of Classification
In order to implement this logic programmatically I used python pandas dataframes to transform the data into a logical structure. The script can be seen here with the explanations of each step outlined. 

 
### Ontological Interpretation of Data
At this point I had the data set I wanted in CSV format ready to import into an ontology. Initially I used Protégé to import my structure via CSV load. This approach worked to a degree and I was able to output the data into RDF/XML format, however it did not satisfy my representation of the data and had serious limitations based on my dataset. Protégé and most ontology software I tried seemed antiquated and not scalable. I chose to use a similar and more pragmatic approach with a Neo4j graph database. This can easily be deployed in the cloud and run to power recommender systems. 

#### Understand the Relationship of RDF and LPG
There is a good article out there about the difference between the two approaches you can read here[https://neo4j.com/blog/rdf-triple-store-vs-labeled-property-graph-difference/]. Ontologies can be imported into Neo4j and represented with LPG. Because of that I chose to bypass explicitly creating my dataset in RDF or OWL and instead use LPG directly. 

#### Importing Data into LPG
I used the script [here] to import data from my csv files into the graph database. Because of the way the data was structured and the frequent occurrence of null values there was a bit of a hack in order to maintain clarity and logical relationships within the database. I’d like to give a shout out to Nicole White for her super informative video[https://vimeo.com/112447027] on how to accomplish this.

#### Graphical View of Data
In order to visualize the relationship between the nodes in the database I’ve provided a few examples below. 
[Example 1]
[Example 2]

#### Programmatic View of Data
##### Querying the Database
