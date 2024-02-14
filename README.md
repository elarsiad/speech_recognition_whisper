# Automatic Speech Recognition with Whisper

This project focuses on building an end-to-end automatic speech recognition (ASR) system using the Whisper model and the Minds14 speech dataset. The goal is to accurately transcribe English speech audio into text transcripts.

## Introduction 

ASR technology allows conversion of human speech into machine-readable text for downstream natural language processing tasks. Recent advancements using deep learning have greatly improved ASR performance across languages.

This project specifically examines using the Whisper self-supervised model from OpenAI for English ASR. As a small-footprint speech encoder, Whisper is well-suited for on-device deployment.

The Minds14 dataset contains over 5,000 utterances labeled into intent classes, providing diverse real-world English speech samples.

## Data

The Minds14 US, AU, and GB English subsets are merged to create a larger training corpus. The data contains:

- Raw speech audio samples at 8kHz sampling rate
- Ground truth orthographic transcripts  
- Intent category labels (not used for core ASR task)

Exploratory analysis includes inspecting the label distribution, audio properties using visualizations, and text normalization.

The data is split 80/20 into train and test sets. For model input, the audio is converted to 16kHz and the text is lowercased with punctuation removed and vocabulary mapped.

## Model Architecture

The Whisper-Tiny model configuration is used, which has:

- 36M parameters
- 80-channel log-mel feature encoder 
- 6 transformer decoder layers
- CNN future predictor

It is initialized from a self-supervised pretraining objective, then fine-tuned to optimize ASR transcript loss using the Minds14 English data.

The Whisper feature extractor, tokenizer, and processor handle audio/text data formatting and batch preparation.

## Training

The model is trained for 1 epoch on Minds14 using the AdamW optimizer. The loss function is label smoothed cross entropy. 
           
Batch size is 4, max sequence lengths are 512 input vectors and 128 output tokens.

## Evaluation

Model predictions on the held-out test set are decoded to text and compared to ground truth using word error rate (WER) between predicted and actual transcripts.

Additionally, subsets are sampled to visualize model performance across speaker accents, utterance lengths, and background noise levels.

## Results

There remains significant room for improvement via training with more epochs, exploring model architectures tailored for speech, and leveraging additional unlabeled speech data.

## Usage

The trained model and notebooks in this repository provide a full pipeline for end-to-end English speech recognition:

1. Load new audio
2. Preprocess with Whisper features 
3. Generate transcript tokens
4. Decode to text