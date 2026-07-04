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
- Amazon EventBridge for event-driven processing
- Amazon CloudFront for global image delivery
- Amazon CloudWatch for logs and monitoring
- AWS CloudFormation for Infrastructure as Code

## Architecture Diagram

![Architecture Diagram](architecture-updated.png)

## Architecture Flow

**Upload flow:**
1. Users authenticate through Amazon Cognito.
2. API Gateway routes requests to Lambda.
3. Lambda generates presigned multipart upload URLs.
4. Images are uploaded to S3 via a Transfer Acceleration endpoint, using multipart upload for large files (up to 100MB) at high concurrency (up to 15k simultaneous uploads).
5. S3 triggers EventBridge when a new image is uploaded.
6. Lambda generates a thumbnail and writes metadata (uploader, filename, public/private flag) to DynamoDB.

**Viewing flow — public images (majority of traffic, ~250k DAU):**
1. CloudFront serves cached images directly from S3 — no auth overhead.

**Viewing flow — private images only:**
1. Users authenticate through Cognito.
2. API Gateway routes to Lambda, which verifies ownership via DynamoDB.
3. CloudFront serves the image using a signed URL/cookie.

## Design Decisions

- **Public vs. private viewing paths are separated** to avoid unnecessary auth overhead for the majority of traffic (public images), while still protecting private content.
- **S3 multipart upload + Transfer Acceleration** handle large file uploads (up to 100MB) reliably at high concurrency
