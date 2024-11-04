# Gustatory-Frame-Extraction
Gustatory information extraction system

## Step 1 - Convert Books

The script in he folder books-converter converts plain texts to the format used for the multitask or the single-task classification. Run the script books_converter.py on the folder containing the documents you want to use to extract frame elements and convert them in a format readable by the classifier. The converter script filters the books by keeping only portions of text (parameter --window) around the seedwords. The SeedLists folder contains the seed lists selected by the author as described in *Paccosi, T., & Tonelli, S. (2024, May). A New Annotation Scheme for the Semantics of Taste. In Proceedings of the 20th Joint ACL-ISO Workshop on Interoperable Semantic Annotation@ LREC-COLING 2024 (pp. 39-46)*. The script doesn't lemmatize, so you need to add all the inflected forms of the seeds to the list.

--folder: The input folder containing the books/document (plain txt, no metadata or tags)

--output: The output folder for the converted documents

--seeds: The file containing the seeds list. E.g. 'SeedLists/seed-taste-pos.txt'

--books: The script allows to merge multible books into a single file, setting the value to 1 create a file for each book. Default value is 100.

--window: The number of sentences to keep around each taste word. 3 means 3 before and 3 after. Default value is 3.

--label: A short label used to assign an ID to the documents (so that later they can be matched with the metadata)

The script creates a -mapping file outside the output folder to map the document ID with the original books.

Usage example:

```python3 books_converter-filter.py --folder books_folder --output output_folder --seeds SeedLists/seed-en-pos.txt --label abc --books 1000```

## Step 2 - Taste prediction 

The folder run-predictions contains the classifier (predict.py) to extract the smell sources from the books converted in the previous step.

Before running the script download the model from here [...] and move it in run-predictions/models folder.

The code has ben tested with python 3.8. To install the required packages, in run-predictions folder run:

```pip install -r requirements.txt```

The script takes as argument in order: model, file to predict, output file (containing the predictions)

Optional: --device to select the gpu to be used. 0 for CUDA based GPUs, 1 for MPS (Apple M1/M2 chips) or -1 for CPU.

The folder test-files contains a sample file to test if the classifier works.

Usage examples:

```python3 predict.py models/en.pt test-files/test-en.tsv predictions/predictions-test-en.tsv --device 0```

The file predictions/sample-predictions-test.tsv shows the correct output to check your system output against.

## Step 3 - Frames Extraction

This extract-annotations.py script in frames-extraction folder extract the predictions from the output of the previous step providing a tsv file with all the frames and sentences.

--folder: the folder with the predictions from the classifier

--output: the output .tsv file

The code has ben tested with python 3.8. To install the required packages, in frames-extraction folder run:

```pip install -r requirements.txt```

Usage example:

```python3 extract-annotations.py --folder ../run-predictions/predictions/ --output test-frames.tsv```

