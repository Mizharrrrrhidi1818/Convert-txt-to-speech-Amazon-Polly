# AWS Text-to-Speech Converter (Project 1)

## Project Overview
This project automates the conversion of text documents (TXT, PDF, newsletters) into speech audio files (MP3). It leverages AWS serverless architecture to process files automatically upon upload.

## Architecture & Services Used
- **Amazon S3**: 
  - `project1-polly-text-files-storage-bucket`: Stores input text files.
  - `project1-polly-audio-files-storage-bucket`: Stores output MP3 audio files.
- **AWS Lambda**: Acts as the processing engine (`Project1PollyTranslationFunction`) that connects the text file to the audio conversion.
- **Amazon Polly**: Converts the extracted text into natural-sounding speech (Voice: Joanna).
- **AWS IAM**: Manages secure access and permissions (`PollyTranslationRole`).

## Estimated Cost
- **Free Tier**: This project can be run entirely within the AWS Free Tier limits.

## Setup Instructions
1. **IAM Role**: Create a role named `PollyTranslationRole` with policies: `AmazonPollyFullAccess`, `AmazonS3FullAccess`, and `AWSLambdaBasicExecutionRole`.
2. **S3 Buckets**: Create the two buckets mentioned above.
3. **Lambda Function**: 
   - Create function `Project1PollyTranslationFunction`.
   - Add S3 Trigger: Bucket `project1-polly-text-files-storage-bucket`, Event `PUT`, Suffix `.txt`.
   - Add Destination: Asynchronous invocation, On failure, S3 `project1-polly-audio-files-storage-bucket`.
   - Deploy the code from `lambda_function.py`.

## Usage
1. Upload a `.txt` file (e.g., `test.txt`) to `project1-polly-text-files-storage-bucket`.
2. The Lambda function will automatically trigger.
3. Once processed, download the generated MP3 file (e.g., `speech-<timestamp>.mp3`) from `project1-polly-audio-files-storage-bucket`.
