# Multi-Label Image Classification and Graphical Ontology

![alt text](/images/resonance.png)

## Introduction

### #IKnowNothing
Given a set of unlabeled images, I set out to create an ontology based on the classification hierarchy from learned image labels. I wanted to make this as visual as possible in order to effectively portray the interrelationships of the image nodes. Using AWS Rekognition, Anaconda, and Neo4j Desktop I was able to create a visual and queryable representation structure of the resonance data set. 

If you wish to just look at the ontology file with the image paths and their classification you can open the result [here](/data_files/product_final.csv). You would need to append the full path prefix to the front to hyperlink out to the actual image if you were to download it. Please continue on though. There is much more to come!

![alt_text](/images/ontology_raw.PNG)

## Table of Contents

1. Introduction
2. Technologies Used
      * 2.1. Final Production
      * 2.2. Development
3.	Procedures and Justifications
     * 3.1. Image Detection
          * 3.1.1. Limitations of Zalando’s and Other Open Source Computer Vision Models
          * 3.1.2. Rekognition Model
               * 3.1.2.1. Misclassification of objects
               * 3.1.2.2. Understanding the Classification Logic
     * 3.2. Inferring the Classification Structure from Logic
          * 3.2.1. Elimination of Noise
          * 3.2.2.1. Super-domain and Sub-domain
          * 3.2.2. Creating a classification structure template
          * 3.2.3. Programmatic Interpretation of Classification
     * 3.3. Ontological Interpretation of Data
          * 3.3.1. Understand the Relationship of RDF and LPG
          * 3.3.2. Importing Data into LPG
          * 3.3.3. Graphical View of Data
4.	Reproduce 
     * 4.1. File Explanation
     * 4.2. Procedure



## Technologies Used
### Final Production
```
Anaconda Python – Jupyter Notebook/AWS CLI
AWS Rekognition – Image Classification
Neo4j – Graph Database
```
### Development
```
Protégé – Ontology Editor
Jena – Ontology Editor
Keras/Tensorflow: Dataflow programming software
```

## Procedures and Justifications
### Image Detection
In order to classify the images into an ontology a computer vision model must first be used to detect objects within each image. The model would need to be trained on a diverse image set such as “ImageSet” and provide multilabel classification as an output. My approach was to use an object detection model in which all objects with a confidence match of greater than 50% would be included. This would effectively identify items within the image that have logical relationships with one another.

#### Limitations of Zalando’s and Other Open Source Computer Vision Models
I initially thought to use a dataset created for identifying popular e-commerce items. Ideally the dataset would have a diverse sampling of fashion items. For this I used Zalando’s FMNIST dataset which provided a set of 60,000 images and 10 category labels. This model had poor performance when used against the Resonance image set and was too simplistic.

#### Rekognition Model
Amazon's Rekognition model provided the comprehensiveness that was needed to identify all objects within the images to a certain degree of accuracy. The software was included under free tier and was assumed to qualify as open source. Using the boto3 API I created a label dataframe locally. The script can be seen [here](/scripts/Product_Meta_Image_Detection.ipynb)

##### Misclassification of objects
Some common misclassifications I noticed after running the model as listed below

1.	Graphics or patterns that contained images such as deer
2.	Sketched Images
3.	Solid colored objects that didn’t show texture

##### Understanding the Classification Logic
The model detected both primary domains and subdomains e.g. skirt and miniskirt. However the model did not create any relationship links between items detected. In the example shown below we see 3 objects. The labels are independant of one another and have no relationship indicators, therefore no hierarchy can be logically assumed from this information. That logic is implemented in the next step.

![alt text](/images/rekognition_sample1.PNG)

### Inferring the Classification Structure 
Initially I wanted to use an NLP model to detect relationships between the labels in order to create a class ordering, but could not find one that fit. My next idea was to use a prepopulated ontology that could infer the structure. Again, there was not one that fit.

In order to create logical order of the image labels I displayed all detected labels grouped by count seen [here](/data_files/grouped_meta.csv). The grouping provided a general understanding of the hierarchical structure used by Rekognition e.g. Human[711]->Female[446]->Girl[1]

#### Elimination of Noise
In order to achieve the above it was obvious that noise labels must be removed to make this a manageable task. Occurrences of 1 that had no relevance to the assumed fashion domain were removed. Many of these were background objects e.g. zebra stripes on asphalt. Additionally, redundant labels such as “Human” and “Person” with the same occurrence counts were reduced to one label.  

##### Super-domain and Sub-domain
The reordering and grouping resulted in 12 super-domains and 7 sub-domains. Within each subdomains there were restrictions as well. Below is an example of that. 

```
Apparel->Female->Clothing->Dress->Miniskirt
```
All the items belong to the super-domain of 1 (Apparel). We realize though that Miniskirt should not be categorized under Dress.
By establishing domain restrictions such that Miniskirt can only belong to Skirt we eliminate pairings such as these. The following would be the result. You can see Miniskirt is eliminated. The super-domain always outweighs the sub-domain.

```
Apparel->Femal->Clothing->Dress
```


#### Creating a classification structure template
The final classification model can be seen [here](/data_files/class_hierarchy.csv)
Below is an subsection example from the file. You can see the logical progression of super-domain and sub-domain represented left to right

![alt text](/images/class_hierarchy.PNG) 

#### Programmatic Interpretation of Classification
In order to implement this logic programmatically I used python pandas dataframes to transform the data into a logical structure. The script can be seen [here](/scripts/Product_Meta_Classification.ipynb) with the explanations of each step commented out. 

 
### Ontological Interpretation of Data
At this point I had the data set I wanted in CSV format ready to import into an ontology. Initially I used Protégé to import my structure via CSV load. This approach worked to a degree and I was able to output the data into RDF/XML format, however it did not satisfy my representation of the data and had serious limitations based on my dataset. Protégé and most ontology software I tried seemed antiquated and not scalable.

I chose to use a similar and more pragmatic approach with a Neo4j graph database. This can easily be deployed in the cloud and often is used to power recomendation systems. It was the clear winner for this implementation. The below graph shows how each image node and it's labels are represented in the graph database. 

![alt text](/images/ImageNodeLabelRelationalDiagram.PNG) 

#### Understand the Relationship of RDF and LPG
There is a good article out there about the difference between the two approaches you can read [here](https://neo4j.com/blog/rdf-triple-store-vs-labeled-property-graph-difference/). Ontologies can be imported into Neo4j and represented with LPG. Because of that I chose to bypass explicitly creating my dataset in RDF or OWL and instead use LPG directly with a CSV import. 

#### Importing Data into LPG
I used the script [here](/scripts/importImageLabelsNeo4j.cypher.txt) to import data from my csv files into the graph database. I’d like to give a shout out to Nicole White for her super informative [video](https://vimeo.com/112447027) on how to accomplish a similar import.

#### Graphical View of Data
In order to visualize the relationship between the nodes in the database I’ve provided a few examples below. You can see the interconnections between the different data nodes. There is the relative path to the image as an ID. If you want to check to see the specific picture simply search using that ID in your local folder where the images are stored. 

The data can be queried to find specific nodes or groupings. You could use this for recommender systems when you want to show similar products. Neo4j uses the cypher query language. 

![alt text](/images/apparel_graph.PNG)
![alt text](/images/multiple_superdomain_graph.PNG)

### Reproduce 
The following instructions will help you to load the ontology CSV into Neo4j

### File Explanation

There are 7 data files and one script file to help import the ontology into Neo4j. Each data file reflects the different level of the hierarchy. Because of null importing it needed to be done in this way. The data files for import can be found [here](/data_files/neo4j_imports/) and the script to import it into Neo4j can be found [here](/scripts/importImageLabelsNeo4j.cypher.txt)

### Procedure
In order to do this follow the instructions below

1. Install the necessary files from above onto your local computer
2. Install Neo4j [here](https://neo4j.com/download/)
3. Create a graph and set the password 
4. "Start" the service
5. Once started click on "Manage" and go to the "Terminal" tab
6. Type cd bin 
7. Type cypher-shell -u "username" -p "password
8. Open the script file downloaded above and paste them into the terminal
4. Once done open Neo4j Browser and you can visualize or query the data as you wish
