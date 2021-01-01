# Serverless Resize Image Service using AWS Services

This Application using Serverless Application Model (SAM).

## How To Deploy

### Deployment diagram

![Alt text](img.jpg?raw=true "Resize Image SAM Diagram")

### Using Docker : Install node_modules

```bash
$ cd src
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
