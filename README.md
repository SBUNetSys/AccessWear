# AccessWear
This is the repository for the MobiCom' 23 paper AccessWear: Making Smartphone Applications Accessible to Blind Users

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
```blind_user_sample_data``` directory contains a subset of blind users' gyroscope gesture data collected using Fossil Gen 5 smartwatch at a 100 Hz sampling rate. It includes 3 samples of 5 different gestures from a user.

Data format: gesture_sample.csv

Gestures are as follows:

1 --> move forearm upwards  

2 --> move forearm horizontally towards right  

3 --> flick wrist  

4 --> move forearm horizontally towards left  

5 --> move forearm downwards
