# 

We will be using Amazon Bedrock  to access foundation models in this workshop.

Below we will configure model access in Amazon Bedrock in order to build and run generative AI applications. Amazon Bedrock provides a variety of foundation models from several providers.

Amazon Bedrock setup instructions
Find Amazon Bedrock by searching in the AWS console.
Search for Bedrock service
1. Select Amazon bedrock
![Screenshot for bedrock.](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/setup/bedrock-search.png)


Expand the side menu.
![Expand the side menu](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/setup/bedrock-menu-expand.png)



From the side menu, select Model access.
Select Model access
![Select Model access](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/setup/model-access-link.png)
Select Model Subnet



Select the "Enable specific model" or "Modifiy model access button.
Select the Manage model
![Select Model Subnet](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/setup/model-access-view-subset.png)

![Select Model Subnet](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/setup/model-access-select.png)


Select the checkboxes listed below to activate the models. If running from your own account, there is no cost to activate the models - you only pay for what you use during the labs. Review the applicable EULAs as needed.


### Amazon (select Amazon to automatically select all Amazon Titan models)
### Anthropic > Claude 3 Sonnet, Claude 
### Meta > Llama 3.1 8B , 70B Instruct
### Mistral AI > 

Click "Next" to activate the models in your account.



Review and submit your models
Select the Manage model
![Select Model Submit](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/setup/model-submit.png)

Select the Manage model

Monitor the model access status. It may take a few minutes for the models to move from In Progress to Access granted status. You can use the Refresh button to periodically check for updates.

Verify that the model access status is Access granted for the previously selected models.

Select the Manage model



![Start Workshop](https://github.com/kaveerh/bedrock-mining-demo/blob/main/content/1Introduction.md "Start Workshop")