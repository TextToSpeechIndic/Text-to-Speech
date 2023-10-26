Complete Installation, Training and Synthesis steps of Vakyansh-TTS

1. Firstly clone the github repository . Follow the link given below:  (git clone )

https://github.com/Open-Speech-EkStep/vakyansh-tts


2. Create anaconda environment with python==3.7
command:
conda create –name vakyansh python==3.7
conda activate vakyansh

3. Install apex . Clone github repo, follow the link given below:

https://github.com/NVIDIA/apex


Enter into apex folder and type some commands:

cd apex
git checkout 37cdaf4
pip install -v --disable-pip-version-check --no-cache-dir ./
cd ../vakyansh-tts

Now in vakyansh-TTS folder , install all the requirements , type command :

pip install -r requirements.txt


After doing all the above steps , installation part will completed . 

Now put your data which is in the form of wav files and corresponding text in csv format (See the dataset format from LJSpeech data).

Your wave files should be in wav folder and have metadata.csv file .
Put both of these in scripts/data folder.

1. Resampling : Changing sampling rate to 22khz of audio files

Now you are in scripts/data folder :
In resample.sh change input_wav_path =’wav’ and type command bash resampe.sh . This will create another folder in scripts/data named as wav_22k containing all audio files with sampling rate of 22khz.

2. Train glow-tts model

move to glow folder , now you are in scripts/glow. Change path of input_text_path (metadata.csv )and input_wav_path (wav_22k folder). 

Run command bash train_glow.sh

3. Vocoder training (HiFiGAN)

Move to hifi folder . scripts/hifi

change input wave path (wav_22k). Run command bash train_hifi.sh

In prepare_data.sh (both in glow and hifi) , you can set the number of valid and test samples accordingly.

4. Inferencing:

Run bash infer.sh command in scripts/inference.
