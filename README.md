# Multi-Cue detector for sign language

This repo contains the code used to train an object detection algorithm to detect human faces and hands as a preprocessing step to a sign language recognition system

## Hand Dataset
The base dataset used to train the hand detector was the [hand dataset](https://www.robots.ox.ac.uk/~vgg/data/hands/).

The dataset labels are in the .mat format and need to be converted into XML in order to train the detector using TensorFlow. To do so we used the ```mat_to_xml.py``` algorithm. To run the code, just use:

```
python3 mat_to_xml.py --img-path /imgs-folder-path --annotations-path /mat-annotations-folder --output-path /folder-to-store-xml
```

After transforming the annotations into XML format, it's necessary to convert then into CSV. This can be easily done by the following command:
```
python3 xml_to_csv.py -i /xml-path -o /csv-output-path
```
Finally, it's necessary to convert the CSV files into TFRecord format to train the object detector using the TF Data API. To do so, just run the following code:
```
python3 generate_tfrecord.py --csv_input=/path-to-csv --output_path ./output.record --img_path=/path-to-images --label_map=./label_map.pbtxt
```
