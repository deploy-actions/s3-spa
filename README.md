# Deploy Single Page Application on S3

## About

This GitHub Action helps deploy your single-page application (SPA) to AWS. It uploads your built files to S3 and delivers them through CloudFront for fast, global content delivery. Based on AWS SAM, it supports custom domain configuration with minimal setup.

## Simple Example of Usage

### Minimum Options

```yml
- name: Configure AWS credentials 🔑
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: ${{ vars.AWS_ROLE_ARN }}
    aws-region: ${{ vars.AWS_REGION }}

- name: Setup Page
  uses: deploy-actions/s3-spa@v1
  with:
    BucketName: simple-spa

- name: Upload Page
  run: aws s3 cp --recursive "." s3://simple-spa
```

### Custom Domain Options

```yml
- name: Configure AWS credentials 🔑
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: ${{ vars.AWS_ROLE_ARN }}
    aws-region: ${{ vars.AWS_REGION }}

- name: Setup Page
  uses: deploy-actions/s3-spa@v1
  with:
    BucketName: simple-spa
    Aliases: www.example.com
    AcmCertificateArn: ${{ vars.ACM_ARN }}
    HostedZoneId: ${{ vars.HostedZoneId }}

- name: Upload Page
  run: aws s3 cp --recursive "." s3://simple-spa
```

## Inputs

| Name              | Description                                                                                                                                                     | Mandatory | Default |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ------- |
| BucketName        | S3 Bucket Name for Single Page Application                                                                                                                      | ✅        |         |
| SAMSourceBucket   | AWS S3 bucket where artifacts referenced in the template are uploaded.                                                                                          |           |         |
| SAMToken          | The GITHUB Authentication token to use for calling the GITHUB Get the latest release API. Defaults to call the API as unauthenticated request if not specified. |           |         |
| Aliases           | AWS::CloudFront::Distribution DistributionConfig Properties, To apply this property, you must also enter `AcmCertificateArn` and `HostedZoneId`.                |           |         |
| AcmCertificateArn | AWS::CloudFront::Distribution ViewerCertificate Properties, To apply this property, you must also enter `Aliases` and `HostedZoneId`.                           |           |         |
| HostedZoneId      | AWS::Route53::RecordSetGroup Properties, To apply this property, you must also enter `Aliases`, `AcmCertificateArn`.                                            |           |         |

## Outputs

| Name           | Description                                                             | Optional |
| -------------- | ----------------------------------------------------------------------- | -------- |
| DistributionId | The distribution's identifier. For example: E1U5RQF7T870K0.             |          |
| DomainName     | The domain name of the resource, such as d111111abcdef8.cloudfront.net. |          |
