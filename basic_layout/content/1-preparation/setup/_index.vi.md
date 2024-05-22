---
title : "Setup Environment"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

To get started, set up the provided workshop application by following the steps:

1. Clone the workshop repository  in your development environment:
    ````
    git clone https://github.com/aws-samples/cloudscape-design-system-workshop.git

    ````
2. Change to the workshop directory:
    ````
    cd cloudscape-design-system-workshop
    ````
3. Install the dependencies by running:
    ```
    npm install
    ```
4. With that explained, you're ready to proceed with the first workshop step.

#### About the workshop repository
All steps towards the final application are available as branches on the repository. If you have local changes, stash them by using git stash or force the check out by adding the -f option to the checkout command.
  - Step 1: Basic layout → Starting the step: ```git checkout step-1```. Get the completed step: ```git checkout - step-1-completed```
  - Step 2: Table view → Starting the step: ```git checkout step-2```. Get the completed step: ```git checkout step-2-completed```
  - Step 3: Creation flow → Starting the step: ```git checkout step-3```. Get the completed step: ```git checkout step-3-completed```
To view the final workshop application, check out the main branch.

#### Clean up
If you want to clean up the files created during the workshop, navigate to the workshop repository folder and delete it.