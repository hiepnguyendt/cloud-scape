---
title : "Step preparation"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> </b> "
---
Before we start, let's prepare our environment for this workshop step.
1. Ensure you're in the cloudscape-design-system-workshop folder.
2. If you have the development server already running, stop that command in your terminal.
3. Check out the step-1 branch by executing:
    ```
    git checkout step-1
    ```

4. Spin up the development server by running:
    ```
        npm run dev
    ```

5. Port 8080 verification.
Verify the output in your terminal to check if the development server is running on port 8080. The output should mention port 8080 as below:

    ```

      VITE v3.2.3  ready in 544 ms
      ➜  Local:   http://localhost:8080/
      ➜  Network: use --host to expose
    
    ```
{{% notice note %}}
**Troubleshooting: 'Port 8080 is already in use'.**\
If the output states ``Port 8080 is in use, trying another one...`` then you need to close the command which uses this port. This could happen when the dev server has stopped and the port did not automatically close. To close the used port, first stop the dev server command followed by closing the process which runs on port 8080 by executing:\
    ```if [[ $(lsof -t -i:8080) ]]; then kill -9 $(sudo lsof -t -i:8080); fi```\
Now, spin up the development server again and do the port 8080 verificatio
{{% /notice %}}

6. Open the `/home/index.html` page we are working on in your browser. The instructions differ depending on the environment where you are running the workshop.
![Preparation](/images/3.png?false&width=90pc)