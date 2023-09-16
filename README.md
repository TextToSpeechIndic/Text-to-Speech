# Complete Installation, Training and Synthesis steps of FastSpeech2

1. Create conda environment python==3.8.10
2. Clone github repository (https://github.com/ming024/FastSpeech2)
3. pip3 install -r requirements.txt (for me Pyworld==0.3)

## For training:
Download data/custom data . Audio in waves folder and transcription in metadata.csv . Put both folder in LJSpeech folder, then run
python3 prepare_align.py config/LJSpeech/preprocess.yaml

It will generate .lab files

For Alignment , we are using MFA (Montreal forced aligner) https://montreal-forced-aligner.readthedocs.io/en/latest/first_steps/index.html#first-steps-align-train-acoustic-model

We do not have pretrained acoustic model for Punjabi , so firstly we trained the acoustic model, for that steps are as follows:

Put Speech corpus having .wav and .lab files in my_corpus/prosodylab_corpus_directory/speaker1 and provide pronunciation dictionary
command: 

mfa validate ~/mfa_data/my_corpus ~/mfa_data/my_dictionary.txt

mfa train ~/mfa_data/my_corpus ~/mfa_data/my_dictionary.txt ~/mfa_data/new_acoustic_model.zip 

It will save the new_acoustic_model.zip in mfa_data folder

Now use this model and Pronunciation dictionary to generate the alignment. 

Again store corpus in same folder , command:
mfa validate ~/mfa_data/my_corpus dictionary.txt new_acoustic_model.zip 

mfa align ~/mfa_data/my_corpus dictionary.txt new_acoustic_model.zip   ~/mfa_data/my_corpus_aligned\

Alignments are generated in the form of textgrid files.

Put TextGrid files in preprocessed_data/LJSpeech/TextGrid/LJSpeech/ and run:

python3 preprocess.py config/LJSpeech/preprocess.yaml

This command generate Energy,Pitch,Duration numpy arrays.Finally run below command to start training

python3 train.py -p config/LJSpeech/preprocess.yaml -m config/LJSpeech/model.yaml -t config/LJSpeech/train.yaml

## For synthesis
python3 synthesize.py --source preprocessed_data/LJSpeech/val.txt --restore_step 900000 --mode batch -p config/LJSpeech/preprocess.yaml -m config/LJSpeech/model.yaml -t config/LJSpeech/train.yaml


This is the complete training process


# Few more resources, check it
https://bootphon.github.io/phonemizer/
This is for text to phonemes conversion 

https://montreal-forced-aligner.readthedocs.io/en/latest/
This is for acoustic model training

https://www.iitm.ac.in/donlab/tts/database.php
From this site , download the dataset, For username and password , do messege me .
