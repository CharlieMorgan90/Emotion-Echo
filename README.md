# Emotion Echo: Emotion Classifier

## Introduction
**Emotion Echo** is an Android app designed to help individuals with autism better recognize emotions in speech through real-time machine learning (ML) detection and an interactive practice mode. Developed using Kotlin and Python, the app features two key modes:

- **Real-Time Mode**:  
  This mode allows users to record audio from their devices. The recorded audio is processed in real time and analyzed by a machine learning model, displaying correlated emotions as an interactive pie chart. The pie chart updates dynamically based on the audio it hears.

- **Practice Mode**:  
  Users can listen to pre-recorded audio clips, guess the emotion, and receive instant feedback.

While I contributed to the app's UI using Kotlin, my primary focus was designing and developing the machine learning models that power the emotion classification. This README delves into the ML model's technical details, techniques, and key performance metrics.  

For more details about the project or instructions to run it locally, refer to the [EmotionEchoLink.md](EmotionEchoLink.md) file.

---

## The Machine Learning Model
The model provided in this repository represents the most advanced version of my work. It prioritizes accuracy on external (real-world) data over performance on the Kaggle dataset used for training and testing. Below are the key components and design decisions behind the model:

### 1. Feature Extraction
- The model processes **Mel Spectrograms** instead of MFCC (Mel Frequency Cepstral Coefficients).  
- While MFCC features work well for speech recognition, **Mel Spectrograms** are more robust for emotion detection due to their ability to better capture the tonal and frequency variations in audio.

### 2. Data Augmentation
To address the limited size of the dataset, I implemented several augmentation techniques to increase diversity:  

**Initial Techniques**:
- **Adding Noise**: Simulates real-world environments with background noise.
- **Time Shifting**: Shifts audio clips slightly forward or backward to account for temporal variations.  

These techniques improved performance on the Kaggle dataset, achieving **98% testing accuracy**.

**Additional Techniques for Generalization**:
- **Pitch Shifting**: Randomly adjusts the pitch of the audio to simulate different vocal ranges.
- **Stretching the Waveform**: Slightly speeds up or slows down the audio to mimic variations in speaking pace.

These additional techniques enhanced the model’s ability to classify unseen, external audio clips, achieving **83% testing accuracy** with better performance on real-world data. Overall, these techniques doubled the dataset size and significantly improved the model's generalization to real-world scenarios.

### 3. Model Architecture
#### Layer Structure:
- **Conv1D Layers**:  
  Two convolutional layers, with 64 and 128 filters respectively, use ReLU activation for non-linearity and dropout (0.2) for regularization to reduce overfitting.
- **Flatten Layer**:  
  Converts the 2D feature maps into a 1D dense representation for the final classification layer.
- **Dense Layer**:  
  A fully connected layer with 8 output neurons and a softmax activation function for multi-class classification across 8 distinct emotions.

#### Performance:
- The model was trained on the Kaggle dataset and achieved an impressive **98% accuracy** on both the testing and validation sets.  
- Additional data augmentation techniques were applied to enhance the model's ability to classify emotions from unseen, external audio clips. This focus on generalization ensures better performance in real-world scenarios.

---

## Issues Encountered

### 1. Compatibility with Android
- TensorFlow and other ML packages used for the model are not directly compatible with Android mobile devices.  
- We had to create a simplified version of the model using lightweight libraries compatible with Android (e.g., TensorFlow Lite).

### 2. Microphone Quality
- The real-time mode's accuracy is highly dependent on microphone quality.  
- For example, my MacBook's microphone performed worse than the microphone on my partner's Android device.  
- Speech variations such as pace, pitch, and accents further complicate emotion classification.

---

## Key Improvements for Future Work

### 1. Dynamic Data Collection
- Add a mode where users record their own voices while prompted with specific emotions (e.g., "Say this sentence in a sad voice").  
- With user permission, these recordings could be stored and labeled for retraining the model periodically.  
- This would increase the diversity of training data over time, addressing the main limitation affecting real-time accuracy.

### 2. Enhanced Real-Time Processing
- Optimize the model's architecture to further reduce latency in real-time mode while maintaining accuracy.


