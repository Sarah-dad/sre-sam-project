# Serverless Resize Image Service using AWS Services

This Application using Serverless Application Model (SAM).

### Deployment diagram

![Alt text](img.jpg?raw=true "Resize Image SAM Diagram")

Here's the process:

1. The user's request the resize operation via POST API request
2. The API Gateway method is configured to trigger a Lambda function to serve the request.
3. The Lambda function gets the original image from the body of the request, resizes it, and uploads the resized image into the bucket with a new name and the new size.
4. When the Lambda function completes, API Gateway outputs body message "ok".
5. The user's can request the now-available resized image from the S3 bucket.

## How To Deploy
### Using Docker : Install node_modules

```bash
$ docker run -it -v "$PWD":/var/task lambci/lambda:build-nodejs12.x /bin/bash
$ npm install
$ exit
```

### Using AWS SAM package
### 1. Build locally

```bash
$ sam build --use-container --skip-pull-image
````

### 2. Package sam template

```bash
$ sam package --output-template-file packaged.yaml --s3-bucket bucket-images-resizer
```

### 3. Deploy

```bash
$ sam deploy --template-file packaged.yaml --stack-name sam-app --capabilities CAPABILITY_IAM
```

## How To Use


```bash
curl --location --request POST 'http://localhost:3000/image' \
--form 'file=@img.jpg' \
--form 's3Key=img.jpg'
```


### Actual output (or where I'm stuck :-( )

The template still need a fix on some permissions, actually I got an access denied while the lambda try to put the "resized image" Object on bucket S3 :

```bash
2021-01-04T09:44:39.784Z	7d70e4b4-bd20-4819-8700-d6c7374be267	INFO	Sending image img.jpg to bucket :  sam-sre-codebucket
2021-01-04T09:44:40.118Z	7d70e4b4-bd20-4819-8700-d6c7374be267	ERROR	Unable to upload :  img.jpg_200  to bucket :  sam-sre-codebucket
    at processTicksAndRejections (internal/process/task_queues.js:97:5)	ERROR	Error: AccessDenied: Access Denied
} body: '{"message":"unable to crop image"}'0-4819-8700-d6c7374be267	INFO	{
END RequestId: 7d70e4b4-bd20-4819-8700-d6c7374be267
```
