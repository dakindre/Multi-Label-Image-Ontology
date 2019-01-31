# Image multi-label classification and fashion ontology

## Issues with Image Classification
1. Graphic clothing vs. improper object detection in photo
2. Drawn Images were often mislabelled

## Image Misclassification Resolution
1. Remove labels with only a single occurance. Sanity check confirmed a majority of the mislabeled data was from detection of background objects

## Example of Good Image Detection
1. Antelope example [4381ac7b5ef0e30f3dd12b7177661fd2] [c5ea12106d62809f7baa57ba551652cb]

## Ontology
1. Structure populated from occurances of labels in dataframe
2. SubClass hierarchy loosely based off Lands End Catalogue Structure
3. By breaking the labels into subclasses and limiting an image to only one item from the subclass I reduced incoorectly labelled items by [Enter Number Here]
