# Image Sharing Platform on AWS

This project designs and deploys a serverless image-sharing platform on AWS using CloudFormation.

## Project Overview

The platform allows users to upload, manage, and share images from web and mobile applications. It supports authentication, private/public image sharing, metadata storage, image processing, and global image delivery.

## AWS Services Used

- Amazon S3 for image storage
- Amazon DynamoDB for image metadata
- Amazon Cognito for user authentication
- AWS Lambda for backend logic and image processing
- Amazon API Gateway for API endpoints
- Amazon CloudFront for global image delivery
- Amazon CloudWatch for logs and monitoring
- AWS CloudFormation for Infrastructure as Code

## Architecture Flow

1. Users authenticate through Amazon Cognito.
2. API Gateway routes requests to Lambda.
3. Lambda generates upload URLs and stores metadata in DynamoDB.
4. Images are uploaded to S3.
5. S3 triggers Lambda when a new image is uploaded.
6. Lambda processes the image and saves the processed version back to S3.
7. CloudFront delivers images globally with low latency.

## CloudFormation Templates

- `platform-s3.yaml`
- `platform-dynamodb.yaml`
- `platform-cognito.yaml`
- `platform-lambda.yaml`
- `platform-s3-trigger.yaml`
- `platform-cloudfront.yaml`
- `platform-api-gateway.yaml`

## Deployment Status

All main stacks were successfully deployed:

- S3: CREATE_COMPLETE
- DynamoDB: CREATE_COMPLETE
- Cognito: CREATE_COMPLETE
- Lambda: CREATE_COMPLETE
- S3 Trigger: CREATE_COMPLETE
- CloudFront: CREATE_COMPLETE
- API Gateway: CREATE_COMPLETE

## Architecture Diagram

![Architecture Diagram](image-plateform.png)

## API Endpoint

API Gateway URL:

```text
https://gkhqadxfwf.execute-api.us-west-1.amazonaws.com/prod
