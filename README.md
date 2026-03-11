# CAN Bus Intrusion Detection using Generative Data Augmentation

## Project Description
This project focuses on improving cybersecurity in modern vehicles by addressing the **class imbalance problem** in Controller Area Network (CAN) intrusion detection datasets. Modern vehicles rely on a communication protocol called CAN which allows multiple Electronic Control Units (ECUs) to exchange messages and control vehicle functionality. However, this protocol lacks essential security mechanisms such as encryption and authentication, making it vulnerable to cyberattacks.

To address this issue, the project explores the use of **Generative AI techniques** to improve the quality of training data used for intrusion detection systems. In real-world automotive datasets, attack samples are extremely rare compared to normal driving data. Because of this imbalance, machine learning models often fail to correctly learn attack patterns. The main objective of this work is to generate realistic synthetic attack data that can balance the dataset and help train more reliable security models.

This repository contains the implementation and experiments carried out to analyze the dataset, identify the imbalance problem, and generate synthetic attack samples using a **Variational Autoencoder (VAE)**.


## Background
Modern vehicles are complex cyber-physical systems that contain multiple embedded computers known as **Electronic Control Units (ECUs)**. These ECUs communicate with each other through the **Controller Area Network (CAN)** protocol in order to control various components such as the engine, braking system, steering, and other vehicle functions.

Although CAN is efficient and widely used in the automotive industry, it was designed at a time when vehicles were not connected to external networks. As a result, the protocol does not include important security features. Messages transmitted through the network are not encrypted and there is no mechanism to verify the identity of the sender.

Because of these limitations, an attacker who gains access to the network can inject malicious messages that may manipulate vehicle functions. Detecting such attacks is an important research problem, and **machine learning based Intrusion Detection Systems (IDS)** are being explored as a potential solution.


## Problem Statement
One of the biggest challenges in building an AI-based intrusion detection system for CAN networks is the **lack of balanced training data**.

In real-world driving scenarios, attack events occur very rarely. Most datasets consist of a very large number of normal CAN messages and only a small number of attack messages. This results in a highly imbalanced dataset where the model primarily learns normal behavior and struggles to recognize attack patterns.

Typical dataset distribution:

- Normal traffic: ~95%  
- Attack traffic: ~5%

If a machine learning model is trained on such data, it may achieve high accuracy simply by predicting every message as normal. However, this defeats the purpose of an intrusion detection system because it fails to identify actual attacks.

To solve this problem, this project focuses on **generating synthetic attack samples** using a deep generative model so that the dataset becomes balanced and more suitable for training AI models.

## Dataset
The project uses the **ROAD (Real Organized Automotive Dataset)** which contains CAN bus traffic collected from a real vehicle during driving sessions.

This dataset provides realistic CAN communication logs captured from an automotive gateway. Compared to simulated datasets, real-world CAN traffic includes signal noise, timing variations, and complex interactions between ECUs, making it more suitable for developing reliable security models.

The dataset includes the following information:

- CAN message timestamp  
- CAN ID (identifier of the ECU sending the message)  
- DLC (Data Length Code)  
- Payload data  
- Attack type or normal label  

## Work Completed So Far

### Phase 1: Data Acquisition and Exploratory Analysis
The first phase of the project focused on understanding and preparing the dataset for further processing.

The raw CAN logs from the ROAD dataset were loaded and examined in order to understand the structure of the data. The preprocessing pipeline involved extracting meaningful features from the CAN frames such as the arbitration ID and the payload data.

Several preprocessing steps were performed, including:

- Parsing hexadecimal CAN frames
- Extracting relevant features from CAN messages
- Organizing the dataset into structured format
- Normalizing numerical values to improve neural network training stability

After preprocessing, an **Exploratory Data Analysis (EDA)** was conducted to analyze the distribution of normal and attack messages in the dataset. The analysis revealed a severe class imbalance problem, where normal traffic dominated the dataset while attack samples represented only a very small fraction.

This confirmed that the dataset was not suitable for direct machine learning training and required data augmentation.

### Phase 2: Generative Data Augmentation
In the second phase of the project, a **Variational Autoencoder (VAE)** was implemented to generate synthetic attack samples.

Traditional oversampling techniques such as SMOTE generate new samples by interpolating between existing data points. However, these methods often fail when dealing with complex and high-dimensional data such as CAN bus traffic. Instead, a generative deep learning model was chosen to better capture the statistical structure of the attack data.

The implemented VAE architecture consists of two main components:

- **Encoder Network**  
  The encoder compresses the input CAN feature vector into a lower-dimensional latent space. Instead of mapping the input to a fixed point, it predicts the parameters of a probability distribution (mean and variance).

- **Decoder Network**  
  The decoder samples from the latent space and reconstructs the original feature vector, effectively generating new synthetic samples that follow the same statistical patterns as the real attack data.

The VAE model was trained using only the **attack class samples** from the dataset. After training, the decoder was used to generate a large number of synthetic attack messages.

Approximately **800,000 synthetic attack samples** were generated and combined with the original dataset.


## Results
After augmentation, the dataset distribution was significantly improved.

Original dataset ratio:

Normal : Attack ≈ 20 : 1

Balanced dataset ratio:

Normal : Attack ≈ 1 : 1

This balanced dataset now provides a much stronger foundation for training machine learning models that can effectively detect cyberattacks in automotive networks.

Additional validation was performed by comparing the statistical distributions of real and synthetic attack samples. The feature distributions showed strong similarity, indicating that the generative model successfully learned the underlying attack patterns.


## Technologies Used
The implementation of this project uses several tools and libraries commonly used in machine learning and data analysis.

- Python
- Google Colab
- NumPy
- Pandas
- Matplotlib
- Deep Learning frameworks (PyTorch / TensorFlow)
- GitHub for version control


## Project Structure
The repository is organized to keep the dataset, models, and experiments structured and easy to understand.

CAN-Intrusion-Detection  
│  
├── data/  
│   └── ROAD dataset files  

├── notebooks/  
│   └── preprocessing and experiments  

├── models/  
│   └── variational autoencoder implementation  

├── results/  
│   └── graphs and analysis outputs  

├── README.md  

└── requirements.txt  


## Future Work
The next phase of this project will focus on building the **actual Intrusion Detection System (IDS)** using the balanced dataset generated in the current phase.

Future tasks include:

- Implementing a **Hybrid CNN-LSTM model**
- Training the model using the balanced dataset
- Detecting both message content anomalies and timing anomalies
- Evaluating the model against different attack scenarios
- Testing the robustness of the detection system under stealth attack conditions

The goal is to develop a reliable and efficient IDS that can detect malicious activity in automotive CAN networks in real time.


## References
- ROAD: Real Organized Automotive Dataset  
- Kingma, D. P., & Welling, M. Auto-Encoding Variational Bayes (ICLR 2014)  
- Miller, C., & Valasek, C. Remote Exploitation of an Unaltered Passenger Vehicle
