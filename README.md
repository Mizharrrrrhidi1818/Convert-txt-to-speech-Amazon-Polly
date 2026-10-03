# AWS Text-to-Speech Converter

## Project Overview
This project automates the conversion of text documents (TXT, PDF, newsletters) into speech audio files (MP3). It leverages AWS serverless architecture to process files automatically upon upload.

## Architecture & Services Used
- **Amazon S3**: 
  - `project1-polly-text-files-storage-bucket`: Stores input text files.
  - `project1-polly-audio-files-storage-bucket`: Stores output MP3 audio files.
- **AWS Lambda**: Acts as the processing engine (`Project1PollyTranslationFunction`) that connects the text file to the audio conversion.
- **Amazon Polly**: Converts the extracted text into natural-sounding speech (Voice: Joanna).
- **AWS IAM**: Manages secure access and permissions (`PollyTranslationRole`).

## Pipeline Design

```mermaid
graph TD
    %% User and Input
    User((User)) -->|1. Uploads Document| S3_Input[(S3 Input Bucket<br/>project1-polly-text-files-storage-bucket)]
    
    %% Event Trigger
    S3_Input -->|2. S3 PUT Event Trigger<br/>| Lambda[AWS Lambda<br/>Project1PollyTranslationFunction]
    
    %% Processing
    Lambda -->|3. Extract Text &<br/>Request Synthesis| Polly[Amazon Polly<br/>Voice: Joanna]
    
    %% Audio Generation
    Polly -->|4. Returns Audio Stream<br/>(MP3)| Lambda
    
    %% Output
    Lambda -->|5. Upload MP3| S3_Output[(S3 Output Bucket<br/>project1-polly-audio-files-storage-bucket)]
    
    %% Error Handling
    Lambda -.->|6. On Failure| S3_DLQ[(S3 Failure Bucket<br/>Dead Letter Queue)]
    
    %% Notification (Pro Feature)
    S3_Output -->|7. Object Created Event| SNS[Amazon SNS<br/>Email Notification]
    SNS -->|8. Your audio is ready!| User
    
    %% Download
    S3_Output -->|9. Download MP3| User
