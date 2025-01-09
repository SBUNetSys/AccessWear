[![DOI](https://zenodo.org/badge/668969913.svg)](https://zenodo.org/badge/latestdoi/668969913)
# AccessWear

This is the repository for the **MobiCom' 23 paper AccessWear: Making Smartphone Applications Accessible to Blind Users**. The repository contains the code and sample data for the blind user gesture recognition pipeline.


AccessWear is a system that improves the accessibility of smartphone touchscreen interactions for blind users using smartwatch gestures. We present the first gesture recognition system that works specifically for blind users which is lightweight
and does not require per-person training. We focus on gesture recognition for forearm and wrist gestures.
AccessWear’s gesture recognition uses Inertial Measurement Unit or IMU sensors of smartwatches. Since human arm and hand movements are primarily rotational we use the gyroscope sensor. Each blind user has a unique style in which they move their arm and hands. Upon observing the gyroscope data of different blind users at a more granular level, we found that despite the overall differences in gesture patterns, each gesture exhibits a distinct signature. This signature, ”nucleus” is the core of the gesture and is consistent across users for the same gesture. We extract this nucleus using a lightweight algorithm. Lastly, we compare the extracted nucleus to a pre-defined template using template-matching techniques. In contrast to existing approaches that require large amounts of data, we have a single template for each gesture to classify a gesture.

<img width="230" alt="accesswear" align="center" src="https://github.com/user-attachments/assets/ff971e63-afd6-4fc7-a7cc-eee8151c5031" />


BibTeX Reference
```
@inproceedings{khanna2023accesswear,
  title={AccessWear: Making Smartphone Applications Accessible to Blind Users},
  author={Khanna, Prerna and Feiz, Shirin and Xu, Jian and Ramakrishnan, IV and Jain, Shubham and Bi, Xiaojun and Balasubramanian, Aruna},
  booktitle={Proceedings of the 29th Annual International Conference on Mobile Computing and Networking},
  pages={1--16},
  year={2023}
}
```

This repository provides a tutorial notebook to understand each step of blind users’ lightweight and no per-person training gesture recognition algorithm.


## 1. Clone this repository
```
git clone https://github.com/SBUNetSys/AccessWear.git
```

## 2. Create a virtual environment
We recommend using conda. Tested on macOS 13.4.1, with Python 3.10.9.

```
conda create -n "accesswear" python=3.10.9
conda activate accesswear
python -m pip install -r requirements.txt
```

Run the jupyter notebook ```gesture_recognition.ipynb```

## Dataset
```blind_user_sample_data``` directory contains a subset of blind users' gyroscope gesture data collected using Fossil Gen 5 smartwatch at a 100 Hz sampling rate. It includes 3 samples of 5 different gestures from a blind user.

Data format: ```gesture_sample.csv```

Gestures are as follows:

1 --> move forearm upwards  

2 --> move forearm horizontally towards right  

3 --> flick wrist  

4 --> move forearm horizontally towards left  

5 --> move forearm downwards


## Experimental Workflow
The ```gesture\_recognition.ipynb``` Jupyter notebook provides step by step description of the lightweight and no per-person training gesture recognition algorithm for blind users. 

First, load the pre-defined nucleus templates obtained from a sighted user. AccessWear does not require training data from each blind participant for the gesture recognition system to work.

Next, load a blind user's gesture data from a CSV file from the dataset folder and pre-process the gyroscope data. 
Change the ```file``` here for experimenting with different gestures:
```
## TODO: change the file name to the file you want 
## to test
file = "blind_user_sample_data/file.csv"
```

We now implement the nucleus detection algorithm.
Each individual has a unique style in which they move their arm and hands, but the core of the gesture remains consistent across users. We call this core signature, "nucleus" of the gesture.
To extract the nucleus, we calculate the RMS energy of the gesture using a sliding window size of 20. To localize the nucleus in the energy signal, we implement a lightweight change point detection algorithm.
We examine the difference in RMSE using windows. When the difference in energy between the reference and the new window exceeds an empirically determined threshold (0.4), the change points are noted. We filter out the change points that are too close to each other.
The nucleus is determined by two change points. The output of this block gives the nucleus position in the signal, which is denoted by ```filtered change points: [x, y]```.

The nucleus of a forearm and a wrist gesture could look similar. We then implement an algorithm to distinguish wrist gestures from forearm gestures. We observe that due to the impulse-like nature of the wrist gesture, jitters are present in the nucleus. To identify these jitters, we implement a lightweight peak detection algorithm. The output of this function sets the boolean ```wrist_bool``` to ```True``` if jitters are detected.

Lastly, we match the detected nucleus against pre-defined gesture templates. We use DTW to match these two signals. The output of this cell prints the detected gesture.
