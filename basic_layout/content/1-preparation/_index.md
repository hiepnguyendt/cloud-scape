---
title : "Preparation"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Before we get to the main content of this workshop, we need to reset the web application.
1. Download the below source code


2. Run the below commands
    ```
    sam build
    sam deploy --guided
    ```

3. Enter the following content:
    - Stack Name []: `fcj-book-shop`
    - AWS Region []: `ap-southeast-1`
    - Confirm changes before deploy [Y/n]: **y**
    - Allow SAM CLI IAM role creation [Y/n]: **y**
    - Disable rollback [y/N]: **y**
    - BooksList may not have authorization defined, Is this okay? [y/N]: **y**
    - BookCreate may not have authorization defined, Is this okay? [y/N]: **y**
    - BookDelete may not have authorization defined, Is this okay? [y/N]: **y**
    - Login may not have authorization defined, Is this okay? [y/N]: **y**
    - Register may not have authorization defined, Is this okay? [y/N]: **y**
    - ConfirmUser may not have authorization defined, Is this okay? [y/N]: **y**
    - Save arguments to configuration file [Y/n]: **y**

4. Open [AWS APIs Gateway console](https://ap-southeast-1.console.aws.amazon.com/apigateway/main/apis?region=ap-southeast-1)


