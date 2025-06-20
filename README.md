# AWS S3 Static Website with Github Actions

## 1. Create S3 Bucket
1. Go to AWS S3 Console
2. Create bucket with name awss3staticgithubaction
3. Enable "Static website hosting" in bucket properties
4. Set index.html as index document

## 2. Set Up GitHub Actions
1. In your GitHub repo, create .github/workflows/deploy.yml:
```
name: Deploy to AWS S3
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-southeast-1
      - run: aws s3 sync . s3://awss3staticgithubaction --delete
```

## 3. Set GitHub Secrets
1. Go to repo Settings → Secrets → Actions
2. Add:
   - AWS_ACCESS_KEY_ID (from IAM user)
   - AWS_SECRET_ACCESS_KEY (from IAM user)

## 4. Deploy
   - Push changes to GitHub, workflow will auto-deploy to S3
   - Access your site at the S3 website endpoint
