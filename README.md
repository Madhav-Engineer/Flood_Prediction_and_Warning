# Rainfall Prediction System using Random Forest Algorithm

This project implements a *rainfall prediction system* using a trained *Random Forest model*. It processes real-time data from sensors connected via serial communication (e.g., from an Arduino), predicts rainfall levels, and updates a Firebase Realtime Database with the results.

## Table of Contents
1. [Introduction](#introduction)
2. [Features](#features)
3. [System Architecture](#system-architecture)
4. [Technologies Used](#technologies-used)
5. [Setup and Configuration](#setup-and-configuration)
6. [Code Explanation](#code-explanation)
7. [Usage](#usage)
8. [Future Enhancements](#future-enhancements)

## Introduction

The Rainfall Prediction System utilizes a *Random Forest model* to predict rainfall levels based on the current day, month, year, and flow rate data from sensors. The predictions are displayed in real-time and updated in a *Firebase Realtime Database*.

This system provides a useful prediction tool that can assist in flood warning systems, agricultural water management, and urban drainage monitoring.

## Features
- Real-time rainfall prediction based on sensor data.
- Utilizes machine learning (Random Forest) for accurate predictions.
- Stores prediction data in *Firebase Realtime Database*.
- User-friendly output showing predicted levels for multiple sensors.

## System Architecture
1. *Sensor Data*: Flow rate data is collected from sensors (e.g., through an Arduino).
2. *Data Processing*: Data is cleaned and prepared for prediction by handling missing values and ensuring the data format is correct.
3. *Machine Learning: The **Random Forest model* processes the data to predict rainfall levels.
4. *Firebase Integration: Predictions are uploaded to a **Firebase Realtime Database* for future access and analysis.

## Technologies Used
- *Python*: Core programming language used for data processing, model integration, and communication with the sensors.
- *Firebase*: Used for real-time database storage of sensor data and predictions.
- *Random Forest*: A machine learning algorithm used for the rainfall prediction model.
- *Serial Communication*: To communicate with Arduino and read sensor data.
- *Joblib*: For model loading and saving.
- *Arduino*: Microcontroller for sensor data collection.
- *Numpy and Pandas*: For data handling and manipulation.

## Setup and Configuration

### Prerequisites
- Python 3.x
- Arduino board and sensors (e.g., flow sensors)
- Firebase account with credentials (Firebase Admin SDK)
- Libraries: firebase-admin, serial, joblib, numpy, pandas
  
### Installation
1. Clone the repository.
    bash
    git clone <repo-url>
    cd Flood_Prediction_and_Warning
    
2. Install the required Python libraries.
    bash
    pip install firebase-admin pyserial joblib numpy pandas
    
3. Set up Firebase:
    - Go to the [Firebase Console](https://console.firebase.google.com/).
    - Create a project and download the ServiceAccountKey.json.
    - Place the ServiceAccountKey.json file in the project root.

4. Upload your trained Random Forest model (rf_model.joblib) to the project directory.

5. Adjust the serial port as per your system's configuration:
    python
    serial_port = 'COM11'  # Change this to the correct port
    

## Code Explanation

### Data Preprocessing
Before the system predicts rainfall, the input data needs to be processed. This involves filling missing values and handling any null data points. Techniques like fillna and deleting rows with null values were applied during this step to ensure the integrity of the dataset.

python
import pandas as pd

# Example of handling missing data
data.fillna(method='ffill', inplace=True)
data.dropna(inplace=True)


### Main Logic
1. *Serial Communication*: Data is collected from sensors using the serial library. Each sensor sends flow rate values, which are parsed and processed.
   
   python
   ser = serial.Serial(serial_port, baud_rate)
   data = ser.readline().decode().strip()
   sensor_data = data.split(',')
   

2. *Model Loading and Prediction*: The Random Forest model is loaded using joblib, and the collected sensor data is used to make predictions for rainfall levels.

   python
   model = joblib.load('rf_model.joblib')
   level1 = model.predict(input_data1)
   level2 = model.predict(input_data2)
   

3. *Firebase Integration*: The predictions are sent to Firebase, allowing real-time data tracking.

   python
   db_ref = db.reference("")
   db_ref.update({
       'date1': day,
       'flow1': flow1,
       'level1': level1[0]
   })
   

## Usage

1. Run the Python script to start real-time data collection and prediction:
    bash
    python rainfall_prediction.py
    

2. The system will:
   - Continuously read sensor data.
   - Predict rainfall levels for each sensor.
   - Update the predictions in Firebase.

## Future Enhancements
- *Improve Model Accuracy*: Explore more advanced machine learning algorithms or fine-tune the existing Random Forest model.
- *Add More Sensors*: Extend the system to accommodate more sensors for wider coverage.
- *Visualize Data*: Implement a web dashboard to visualize the predicted rainfall data in real time.
