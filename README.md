# Multi-Cue detector for sign language

This repo contains the code used to train an object detection algorithm to detect human faces and hands as a preprocessing step to a sign language recognition system

## Hand Dataset
The initial dataset used to train the hand detector was the [hand dataset](https://www.robots.ox.ac.uk/~vgg/data/hands/).

The dataset labels are in the .mat format and need to be converted into XML in order to train the detector using TensorFlow. To do so we used the ```mat_to_xml.py``` algorithm. To run the code, just use:

```
python3 utils/mat_to_xml.py --img-path /imgs-folder-path --annotations-path /mat-annotations-folder --output-path /folder-to-store-xml
```

After transforming the annotations into XML format, it's necessary to convert then into CSV. This can be easily done by the following command:
```
python3 utils/xml_to_csv.py -i /xml-path -o /csv-output-path
```
Finally, it's necessary to convert the CSV files into TFRecord format to train the object detector using the TF Data API. To do so, just run the following code:
```
python3 utils/generate_tfrecord.py --csv_input=/path-to-csv --output_path ./output.record --img_path=/path-to-images --label_map=./utils/label_map.pbtxt
```

## Hand and Face Dataset
Once the goal of the most recent work is to detect hand and face simultaneously, another dataset was required, since it's necessary to have hands and faces annotated at the same time. For this pourpose, we used the "Autonomy" dataset, which contains a collection of other open source datasets but with both hand and face annotated. The original paper can be found [here](https://autonomy.cs.sfu.ca/doc/mohaimenian_iros2018.pdf).

The annotations are found in TXT format, and could be converted to XML by using the **convert_txt_to_xml.py** script. To run the code, first it's necessary to change the dataset path in the beginning of the script and the label map with the class annotations. After that, just run the following command.
```
python3 utils/convert_txt_to_xml.py
```
This code was got from [this](https://github.com/MuhammadAsadJaved/Important-shells) open-source repo.

**Note:** the "Autonomy" folder of this dataset is structured in multiple folders. To convert the annotations, first I used the script entitled "move_files.py" to centralize all the images and annotations.

After that, it's also necessary to convert the XML annotations and the images into TFRecords. To do so, first we need to centralize all the data in the train and test folders. It can be done by running the following code:
```
python3 utils/dataset_split.py
```
The default division proportion is 80/20, respectivelly. 

