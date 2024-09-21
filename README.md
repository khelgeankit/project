# project
name: Deploy to AWS Elastic Beanstalk

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.x'

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Deploy to Elastic Beanstalk
      uses: einaregilsson/beanstalk-deploy@v20
      with:
        application_name: flask-aws-app
        environment_name: flask-aws-app-env
        version_label: ${{ github.sha }}
        region: us-east-1  # Change to your region
        zip_file: ./flask-aws-app.zip
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
