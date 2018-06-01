
## Installation 

### Preface: Requirements
Mac OS X or Linux. 

* Required: 
	* Python 3.5
	* numpy
	* keras
	* tensorflow 
	* librosa
	* matplotlib
	* h5py

	
...the `requirements.txt` file method is going to try to install both required and optional packages.

### Installation:
`git clone https://github.com/sap9433/panotti.git`

`cd sound-classification-with-tf-keras`

`pip install -r requirements.txt`



## Quick Start
* This repo comes with Audio samples to predict baby cry and coungh .
* run `python preprocess_data.py`
* run `python train_network.py`
* run `python eval_network.py`  - This applies the trained network to the testing dataset and gives you accuracy reports.


### Data organization:
Sound files should go into a directory called `Samples/` that is local off wherever the scripts are being run.  Within `Samples`, you should have subdirectories which divide up the various classes.

(Within each subdirectory of `Samples`, there are loads of .wav or .mp3 files that correspond to each of those classes.)

Dowload this https://www.dropbox.com/s/wuykvh5krp2jq7n/Samples.zip?dl=0 . Place it under the root folder


### Data augmentation & preprocessing:

#### (Optional) Augmentation:

The "augmentation" will [vary the speed, pitch, dynamics, etc.](https://bmcfee.github.io/papers/ismir2015_augmentation.pdf) of the sound files ("data") to try to "bootstrap" some extra data with which to train.  If you want to augment, then you'll run it as

`$ python augment_data.py <N>  Samples/*/*`

where *N* is how many augmented copies of each file you want it to create.  It will place all of these in the Samples/ directory with some kind of "_augX" appended to the filename (where X just counts the number of the augmented data files).
For augmentation it's assumed that all data files have the same length & sample rate.


#### (Required) Preprocessing:
When you preprocess, the data-loading will go *much* faster (e.g., 100 times faster) the next time you try to train the network. So, preprocess.

Preprocessing will pad the files with silence to fit the length to the length of the longest file and the number of channels to the file with the most channels. It will then generate mel-spectrograms of all data files, and create a "new version" of `Samples/` called `Preproc/`.

It will do an 80-20 split of the dataset, so within `Preproc/` will be the subdirectories `Train/` and `Test/`. These will have the same subdirectory names as `Samples/`, but all the .wav and .mp3 files will have ".npy" on the end now.  Datafiles will be randomly assigned to `Train/` or `Test/`, and there they shall remain.

To do the preprocessing you just run

`$ python preprocess_data.py`


## Training  the Network
`$ python train_network.py`

That's all you need. 

It will perform an 80-20 split of training vs. testing data,  

It's set to run for 20 epochs, feel free to shorten that or just ^C out at some point.  It automatically does checkpointing by saving(/loading) the network weights via a new file `weights.hdf5`, so you can interrupt & resume the training if you need to.


## Evaluating the Network

`$ python eval_network.py`

Will give some validation scores along the way. 


## Extra Tricks
 python ./predict_class.py file /Users/diesel/Documents/sound-classification-with-tf-keras/Samples/Cry/baby-crying-01_aug1.wav

