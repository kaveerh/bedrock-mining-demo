---
title : "AWS Cloud9 setup"
weight : 7
---
We will be using [AWS Cloud9](https://aws.amazon.com/cloud9/)  as our integrated development environment for this workshop. AWS Cloud9 is one option for building applications with Amazon Bedrock - you can also use your own development tools (VS Code, PyCharm, etc.), [Amazon SageMaker Studio](https://aws.amazon.com/sagemaker/studio/) , or Jupyter Notebooks.

Below we will configure an AWS Cloud9 [enviroment](https://docs.aws.amazon.com/cloud9/latest/user-guide/environments.html)  in order to build and run generative AI applications. An environment is a web-based integrated development environment for editing code and running terminal commands.

::::alert{header="Assumptions for the following instructions"}
- AWS Cloud9 will be run from the same account and region where Bedrock foundation models have been enabled.
- The acccount and region have a default VPC configured (this is the AWS default).
- If you have any challenges below, you may need to access Bedrock from your desktop environment, or create an alternate configuration.
::::

# AWS Cloud9 setup instructions
1. In the AWS console, select the region that has Amazon Bedrock foundation models enabled.
 
![Sign in](/static/img/get-started/region-selection.png)

2. In the AWS console, search for Cloud9.
   - Select Cloud9 from the search results.
   
![Expand the side menu](/static/cloud9/search-cloud9.png)

3. Select Create environment.

![Expand the side menu](/static/cloud9/cloud9-welcome.png)

4. Set the environment details
   - Set Name to :code[bedrock-environment]{showCopyAction=true}.

![Expand the side menu](/static/cloud9/environment-details.png)

5. Set the EC2 instance details.
   - Set Instance type to t3.small
   - Set Platform to Ubuntu Server 22.04 LTS
   - Set Timeout to 4 hours

![Expand the side menu](/static/cloud9/cloud9-ubuntu-instance.png)


::::alert{type="warning" header="Did you select Ubuntu as your platform?"}
Please double-check that you set Platform to Ubuntu Server 22.04 LTS. This ensures that you will have a Python version that can support LangChain and other critical libraries.
::::

6. Select the Create button.

![Expand the side menu](/static/cloud9/create-button.png)

7. Wait for the environment to be created.
   - You should get a "Successfully created bedrock-environment" message in the top banner when ready.
   - In the Environments list, click the Open link. This will launch the AWS Cloud9 IDE in a new tab.


::::alert{header="Handling environment creation errors"}
If you get an error message about the selected instance type not being available in the availability zone, delete the environment. Try provisioning again with a different size instance type.
::::


![Expand the side menu](/static/cloud9/cloud9-ready.png)

8. Confirm that the AWS Cloud9 environment loaded properly.
   - You can close the Welcome tab
   - You can drag tabs around to the position you want them in.    

![Expand the side menu](/static/cloud9/cloud9-ide.png)

In this example, the bash terminal tab was dragged up to the top tab strip, and the bottom-aligned panel was closed:

![Expand the side menu](/static/cloud9/cloud9-panels.png)

::::alert{type="success" header="Congratulations!"}
You have successfully configured and launched AWS Cloud9!
::::

When you have completed the labs and are done using AWS Cloud9, you can delete your AWS Cloud9 environment by following the [clean-up instructions](/clean-up/).