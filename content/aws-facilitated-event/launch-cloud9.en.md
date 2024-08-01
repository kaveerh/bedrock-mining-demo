---
title : "Launch AWS Cloud9"
weight : 4
---
We will be using [AWS Cloud9](https://aws.amazon.com/cloud9/)  as our integrated development environment for this workshop. AWS Cloud9 is one option for building applications with Amazon Bedrock - you can also use your own development tools (VS Code, PyCharm, etc.), [Amazon SageMaker Studio](https://aws.amazon.com/sagemaker/studio/) , or Jupyter Notebooks.

Below we will configure an AWS Cloud9 [enviroment](https://docs.aws.amazon.com/cloud9/latest/user-guide/environments.html)  in order to build and run generative AI applications. An environment is a web-based integrated development environment for editing code and running terminal commands.

# AWS Cloud9 setup instructions
1. In the AWS console, search for Cloud9.
   - Select Cloud9 from the search results.
 
 ![Search for cloud9 service](/static/cloud9/search-cloud9.png)

2. In the Environments list, click the Open link. This will launch the AWS Cloud9 IDE in a new tab.
   
 ![Expand the side menu](/static/cloud9/open-cloud9.png)

3. Confirm that the AWS Cloud9 environment loaded properly.
    - You can close the Welcome tab
    - You can drag tabs around to the position you want them in.
 
 ![Select Model access](/static/cloud9/cloud9-ide.png)

In this example, the bash terminal tab was dragged up to the top tab strip, and the bottom-aligned panel was closed:
   
 ![Select the Manage model](/static/cloud9/cloud9-panels.png)

4. Verify configuration by pasting and running the following into the AWS Cloud9 terminal:
:::code{showCopyAction=true showLineNumbers=false language=bash}
cd ~/environment/
python ~/environment/workshop/completed/api/bedrock_api.py
:::

If everything is working properly, you should see a response about Manchester, New Hampshire:

 ![test Bedrock](/static/cloud9/test.png)

::::alert{type="success" header="Congratulations!"}
You have successfully launched AWS Cloud9!
::::