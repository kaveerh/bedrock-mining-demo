---
# Safety and Risk Management"

---


Welcome to our workshop on Bedrock's knowledge base a safety chatbot. In this workshop, we'll explore how to harness the power of Bedrock's capabilities to create a chatbot tailored specifically for safety regulations around the mining sector.

Let's dive in!

# What is Retrieval-Augmented Generation?
Retrieval-Augmented Generation (RAG) is the process of optimizing the output of a large language model, so it references an authoritative knowledge base outside of its training data sources before generating a response. Large Language Models (LLMs) are trained on vast volumes of data and use billions of parameters to generate original output for tasks like answering questions, translating languages, and completing sentences. RAG extends the already powerful capabilities of LLMs to specific domains or an organization's internal knowledge base, all without the need to retrain the model. It is a cost-effective approach to improving LLM output so it remains relevant, accurate, and useful in various contexts.

# How can AWS support your Retrieval-Augmented Generation requirements?
Amazon Bedrock is a fully-managed service that offers a choice of high-performing foundation models—along with a broad set of capabilities—to build generative AI applications while simplifying development and maintaining privacy and security. With knowledge bases for Amazon Bedrock, you can connect FMs to your data sources for RAG in just a few clicks. Vector conversions, retrievals, and improved output generation are all handled automatically.

# What task are we performing in the lab
In this lab example we will use the Large Language model to query a Mine Safety document and find the relavent information base on our prompt

##  Step 1

Select Knowledge base from the left navigation pane under Orchestration.

![Select KB](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/select-kb.png )


## Step 2

Select Chat with your document

![Chat with your document](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/Bedrock-kbselect.png )

## Step 3
Select the Model 
![Select a Model ](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/kb-modelselect)
![Select a Model picks ](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/kb-modelpick.png)

## Step  4 
Download the following file to you PC
![Sample Mine SafetyDocument](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/MineHealthandSafetyAct29-small.pdf "Sample Mine SafetyDocument")

Choose a Document 
![Choose a Document](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/Kb-fileselect.png )


## Step 5

Copy the Prompt and paste it in the windows.

```bash
Who should pay for the cost of Annual medical reports
```

![Chat with your document](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/kb-prompt.png )

## Sample Result 1 

![Sample result1](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/result1.png )


## Step 6



```bash
What can we do to Enhancing effectiveness of investigation
```
## Sample Result 2

![Sample result 2](https://github.com/kaveerh/bedrock-mining-demo/blob/main/static/safety/result2.png )









