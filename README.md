# Mulit-Label Image Classification and Fashion Ontology

## Issues with Image Classification
1. Graphic clothing vs. improper object detection in photo
2. Drawn Images were often mislabelled
3. Since I chose to classify items by strongest confidence in a folder, folders that have weak confidence for the primary image but strong confidence for a secondary item would be mislabeled. See below sample of [mislabled shirt as skirt based]
4. Images with white items or outline drawings were not well classified
5. Articles of clothing that were solid colors and didn't show texture well were often mislabeled

## Image Misclassification Resolution
1. Remove labels with only a single occurance. Sanity check confirmed a majority of the mislabeled data was from detection of background objects
2. A way to resolve these issues is by identifying these misrepresentations and then creating correctly labeled samples and to retrain the CNN model to properly identify them. Because I chose to use a model that I can not fine tune it would mean I would need to go and recreate a model that could learn from these samples. Due to time constraints it was not possible. 
3. Things that were removed as abstract or too detailed

  Granular Human Characteristics such as head, hair, beard, blonde, Tattoo
  Obvious incorrect mislabelings such as galaxy, interstellar
  The word "Rug" occured frequently and had a high confidence rating outperforming actual apparel

## Example of Good Image Detection
1. Antelope example [4381ac7b5ef0e30f3dd12b7177661fd2] [c5ea12106d62809f7baa57ba551652cb]

## Ontology
1. Structure populated from occurances of labels in dataframe
2. SubClass hierarchy loosely based off Lands End Catalogue Structure
3. By breaking the labels into subclasses and limiting an image to only one item from the subclass I reduced incoorectly labelled items by [Enter Number Here]
