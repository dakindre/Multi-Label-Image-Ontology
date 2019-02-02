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

<ul class="graph-diagram-markup" data-internal-scale="1" data-external-scale="1">
  <li class="node" data-node-id="0" data-x="-1117.2548828125" data-y="-573.6793823242188">
    <span class="caption">Object1</span><dl class="properties"><dt>{name</dt><dd>Female}</dd></dl></li>
  <li class="node" data-node-id="1" data-x="-1117.2548828125" data-y="-320.626220703125">
    <span class="caption">ObjectType2</span><dl class="properties"><dt>{name</dt><dd>Clothing}</dd></dl></li>
  <li class="node" data-node-id="2" data-x="-1323.266357421875" data-y="-172.37808227539062">
    <span class="caption">ObjectType3</span><dl class="properties"><dt>{name</dt><dd>Shirt}</dd></dl></li>
  <li class="node" data-node-id="3" data-x="-1368.6716690063477" data-y="-437.51624298095703">
    <span class="caption">Image</span><dl class="properties"><dt>{image_url</dt><dd>0006f69d1aadc924d0f81922644c584b\image_1.jpg}</dd></dl></li>
  <li class="node" data-node-id="4" data-x="-1323.266357421875" data-y="-679.3394165039062">
    <span class="caption">Object</span><dl class="properties"><dt>{name</dt><dd>Apparel}</dd></dl></li>
  <li class="node" data-node-id="5" data-x="-1585.0989990234375" data-y="-200.6033172607422">
    <span class="caption">ObjectType4</span><dl class="properties"><dt>{name</dt><dd>Cap}</dd></dl></li>
  <li class="node" data-node-id="6" data-x="-1680.0989990234375" data-y="-437.51624298095703">
    <span class="caption">ObjectType5</span><dl class="properties"><dt>{name</dt><dd>Plaid}</dd></dl></li>
  <li class="node" data-node-id="7" data-x="-1585.0989990234375" data-y="-637.6033325195312">
    <span class="caption">ObjectType6</span><dl class="properties"><dt>{name</dt><dd>Tartan}</dd></dl></li>
  <li class="relationship" data-from="3" data-to="4">
    <span class="type">Has</span>
  </li>
  <li class="relationship" data-from="3" data-to="0">
    <span class="type">Has</span>
  </li>
  <li class="relationship" data-from="3" data-to="1">
    <span class="type">Has</span>
  </li>
  <li class="relationship" data-from="3" data-to="2">
    <span class="type">Has</span>
  </li>
  <li class="relationship" data-from="3" data-to="5">
    <span class="type">Has</span>
  </li>
  <li class="relationship" data-from="3" data-to="6">
    <span class="type">Has</span>
  </li>
  <li class="relationship" data-from="3" data-to="7">
    <span class="type">Has</span>
  </li>
  <li class="relationship" data-from="0" data-to="4"></li>
  <li class="relationship" data-from="1" data-to="0"></li>
  <li class="relationship" data-from="2" data-to="1"></li>
  <li class="relationship" data-from="5" data-to="2"></li>
  <li class="relationship" data-from="6" data-to="5"></li>
  <li class="relationship" data-from="7" data-to="6"></li>
</ul>
