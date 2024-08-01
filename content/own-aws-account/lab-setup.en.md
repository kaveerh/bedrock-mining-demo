---
title : "Lab setup"
weight : 8
---

# Download and configure the assets for the labs

1. In the AWS Cloud9 IDE, select the bash terminal
  
![Sign in](/static/cloud9/cloud9-terminal.png)

2. Paste and run the following into the terminal to download and unzip the code.

:::code{showCopyAction=true showLineNumbers=false language=bash}
cd ~/environment/
curl 'https://ws-assets-prod-iad-r-pdx-f3b3f9f1a7d6a3d0.s3.us-west-2.amazonaws.com/4db05a4c-f22f-4d24-8800-932626a8c197/workshop.zip' --output workshop.zip
unzip workshop.zip -d workshop
:::

Once completed, you should see the unzip results in the terminal:

3. Install the dependencies for the labs.

![Sign in](/static/cloud9/zip.png)

:::code{showCopyAction=true showLineNumbers=false language=bash}
pip3 install -r ~/environment/workshop/setup/requirements.txt -U
:::

If everything worked properly, you should see a success message (you can disregard a warning like below):

![Sign in](/static/cloud9/reqs.png)

4. Verify configuration by pasting and running the following into the AWS Cloud9 terminal:

:::code{showCopyAction=true showLineNumbers=false language=bash}
cd ~/environment/
python ~/environment/workshop/completed/api/bedrock_api.py
:::

If everything is working properly, you should see a response about Manchester, New Hampshire:

![Sign in](/static/cloud9/test.png)

5. Paste and run the following into the terminal to download Quicksight with Amazon Q lab files.

:::code{showCopyAction=true showLineNumbers=false language=bash}
cd ~/environment/
curl 'https://ws-assets-prod-iad-r-lhr-cc4472a651221311.s3.eu-west-2.amazonaws.com/4db05a4c-f22f-4d24-8800-932626a8c197/iotdata.csv' --output iotdata.csv && 
curl 'https://ws-assets-prod-iad-r-lhr-cc4472a651221311.s3.eu-west-2.amazonaws.com/4db05a4c-f22f-4d24-8800-932626a8c197/manifest.json' --output manifest.json
:::

6. Upload the iotdata.csv to S3 bucket and replace *BUCKETNAME* in the manifest.json with the created bucket name

---
::::alert{type="success" header="Congratulations!"}
You may now proceed to the labs.
::::

When you have completed the labs and are done using AWS Cloud9, you can delete your AWS Cloud9 environment by following the [clean-up instructions](/clean-up/).