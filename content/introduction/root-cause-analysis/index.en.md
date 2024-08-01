---
title : "Root Cause Analysis of Equipments"
weight : 26
---
Root Cause Analysis of Equipments Using Manufacturer specs

# Lab introduction
---
Final product:

:image[synthetic image generation]{src="/static/root-cause-analysis/demo.gif" width=900}

In this lab, we will build a simple text generator with [Amazon Bedrock](https://aws.amazon.com/bedrock/) , [Textract](https://aws.amazon.com/textract), and Streamlit. We will upload an electric motor datasheet specs document, extract specs, pass it to Bedrock, pass sensor data and return the foundation model’s response. While this is a fairly trivial example, it allows us to understand how to build a basic generative AI prototype with very little code.

You can build the application code by copying the code snippets below and pasting into the indicated Python file.

---
# Use cases
Incorporating GenAI into your RCA process, you can make the analysis more adaptive, context-aware, and capable of reasoning his includes data sheets, technical manuals, and any other documentation that outlines the specifications and operational parameters is good for the following use cases:

- Understand normal operating conditions then identify deviations from specifications
- Accurate and efficient identification of root causes in electric motor issues

You can build the application code by copying the code snippets below and pasting into the indicated Python file.
::::alert{header="Just want to run the app?"}
[You can jump ahead to run a pre-made application](#run-the-streamlit-app).
::::

---
# Architecture
:image[synthetic image generation]{src="/static/root-cause-analysis/intro.jpg" width=800}

:image[synthetic image generation]{src="/static/root-cause-analysis/arch.png" width=800}

This application consists of two files: one for the Streamlit front end, and one for the supporting library to make calls to Bedrock.

---
# Create the library script
---
First we will create the supporting library to connect the Streamlit front end to the Bedrock back end.

1. Navigate to the workshop/labs/root-cause-analysis folder, and open the file rca_lib.py

:image[synthetic image generation]{src="/static/root-cause-analysis/rca_lib.png" width=900}

2. Add the import statements.
  - These statements allow us to use Bedrock, Textract, Anthropic and data type.
  - You can use the copy button in the box below to automatically copy its code:
:::code{showCopyAction=true showLineNumbers=false language=python}
import json
import boto3
import textract
import anthropic
from typing import Dict
:::

3. Add these functions to call Bedrock.
  - We are creating three functions to call different Bderock FMs. We may call Bedrock Claude Instant, Bedrock Claude 2, and Bedrock Titan via the Streamlit front end application. This method creates a Bedrock client and then passes the input content to Bedrock.
  - Each function passes the custom prompt to Bedrock.
:::code{showCopyAction=true showLineNumbers=false language=python}
session = boto3.Session()

bedrock = session.client(service_name='bedrock-runtime') #creates a Bedrock client

def call_bedrock_claude_2(prompt_text, max_tokens_to_sample=1024):
    body = {
        "prompt": anthropic.HUMAN_PROMPT+prompt_text+anthropic.AI_PROMPT,
        "max_tokens_to_sample": max_tokens_to_sample
    }
    return invoke_model("anthropic.claude-v2", body)['completion']

def call_bedrock_claude_instant(prompt_text, max_tokens_to_sample=1024):
    body = {
        "prompt": anthropic.HUMAN_PROMPT+prompt_text+anthropic.AI_PROMPT,
        "max_tokens_to_sample": max_tokens_to_sample
    }  
    return invoke_model("anthropic.claude-instant-v1", body)['completion']

def call_bedrock_titan(prompt_text, max_token_count=3072, temperature=0.7, top_p=1, stop_sequences=[]):
    body = {
        "inputText": prompt_text,
        "textGenerationConfig": {
            "maxTokenCount": max_token_count,
            "stopSequences": [],
            "temperature": temperature,
            "topP": top_p,
        }
    } 
    return invoke_model("amazon.titan-text-express-v1", body)['results'][0]['outputText']

def invoke_model(model_id, body):
  response = bedrock.invoke_model(
    modelId=model_id,
    contentType="application/json",
    accept="application/json", 
    body=bytes(json.dumps(body), 'utf-8')
  )

  return json.loads(response['body'].readlines()[0])

def call_bedrock(model, prompt_text):
    func = models[model]
    return func(prompt_text).replace("$","\$")

:::

4. Add these functions to get motor specifications summary.
  - We are creating a function to parse the PDF file and extract it as string using Extract.
  - The function then passes the extracted text to prompt Bedrock model.
:::code{showCopyAction=true showLineNumbers=false language=python}
def upload_file_get_summary(file_name, model):
    motor_contents = process_file(file_name, model)
    prompt_text = 'Describe the motor specifications based on the raw data below:\n'+ motor_contents
    
    summary = call_bedrock(model, prompt_text).replace("$","\$")
    return motor_contents, summary 

def process_file(file_name, model):
    contents = textract.process(file_name).decode('utf-8')
    chunk = char_limits[model]
    motor_contents = contents[:chunk].replace('$','\$')
    return truncate_to_words(motor_contents)

def truncate_to_words(content, max_words=800):
    words = content.split()
    return ' '.join(words[:max_words])

models: Dict[str, callable] = {
  "bedrock titan": call_bedrock_titan,
  "bedrock claude instant": call_bedrock_claude_instant,
  "bedrock claude 2": call_bedrock_claude_2
}

char_limits: Dict[str, int] = {
  "bedrock titan": 4000,  
  "bedrock claude 2": 10000,
  "bedrock claude instant": 10000
}
:::
1. Save the file.

Great! You are done with the backing library. Now we will create the front-end application.

---
# Create the Streamlit front-end app
1. In the same folder as your lib file, open the file rca_app.py
2. Add the import statements.
   - These statements allow us to use Streamlit elements and call functions in the backing library script.

:::code{showCopyAction=true showLineNumbers=false language=python}
import os
import streamlit as st
import rca_lib as rcalib #reference to local lib script
:::

3. Add the motor options, Bedrock FMs and initialize the page sessions.

:::code{showCopyAction=true showLineNumbers=false language=python}
motor_options = {
  "AC motor 6W 110-120VAC": "Transmotec-Datasheet-AI-006W.pdf",
  "RS PRO 3 Phase AC motor": "A700000006779781.pdf", 
  "AMA-IE2 90L2 2.2kW 2 Pole": "AMA-IE2 90L2 (B5).pdf"
}

fms = ['Bedrock Titan', 'Bedrock Claude Instant', 'Bedrock Claude 2']
default_model = fms.index('Bedrock Claude Instant')

default_values = {
  "img_summary": None,
  "motor_summary": None,
  "motor_contents": None, 
  "label_text": None,
  "generated_code": None,
  "selected_motor": None
}

def initialize_session_state():
    for key, value in default_values.items():
        if key not in st.session_state:
            st.session_state[key] = value
:::

4. Add the page title, configuration and description.
   - Here we are setting the page title on the actual page and the title shown in the browser tab.  

:::code{showCopyAction=true showLineNumbers=false language=python}

def configure_sidebar():
    return st.sidebar.selectbox('Select a FM', options=fms, index=default_model)

def configure_page():
  st.set_page_config(
    page_title="GenAI IIoT Electric Motor Diagnosis", 
    page_icon="cloud_with_lightning"
  )

def add_header():
  st.title("GenAI IIoT Electric Motor Diagnosis")
  st.markdown("# :orange[Root Cause Analysis] of Electric Motors Using Manufacturer Specs")
  st.sidebar.header("GenAI IIoT Electric Motor Diagnosis")

def add_description():
  st.markdown("### With this demo you can:")

  st.write("1. Upload an electric motor datasheet in pdf format")
  st.markdown("- [RS PRO 3 Phase AC motor](https://docs.rs-online.com/7939/A700000006779781.pdf)")
  st.markdown("- [AMA-IE2 90L2 2.2kW 2 Pole](https://amtecs.co.uk/datasheet/6Gq4LEgn7OAWDRKvdJ53)")
  st.markdown("- [AC motor 6W 110-120VAC](https://transmotec.com/Download/Datasheets/Transmotec-Datasheet-AI-006W.pdf)")
  st.markdown('''
  <style>
  [data-testid="stMarkdownContainer"] ul{
    list-style-position: inside;
  }
  </style>
  ''', unsafe_allow_html=True)

  st.write("2. Generate examples of IIoT sensor data representing anomalous and optiomal conditions based on motor specs.")
  st.write("3. Evaluate IIoT sensor data to determine anomalies and potential root causes.")
:::

5. Add the input elements.
   - We are creating a dropdown box with predefined motors specs to get the user's motor spec and send it to Bedrock.
   - Upload a file and call the rca library to generate summary

:::code{showCopyAction=true showLineNumbers=false language=python}
def select_motor():
  c1, c2 = st.columns(2)
  
  c1.subheader("Choose a Motor Spec")
  selected_motor = c1.selectbox("Select motor", list(motor_options.keys()))

  return motor_options[selected_motor]

def upload_file(selected_file, model):
  if selected_file is None or st.session_state['selected_motor'] == selected_file: return

  st.session_state["selected_motor"] = selected_file
  with st.spinner("Processing..."):
    file_path = os.path.join("../../data/root-cause-analysis", selected_file)
    motor_contents, motor_summary = rcalib.upload_file_get_summary(file_path, model)
    st.session_state["motor_contents"] = motor_contents
    if len(motor_summary) > 5:
        st.session_state["motor_summary"] = motor_summary  
        st.success("File uploaded and processed")
:::

6. Add the motor specs summary and input to capture sensor data.
   - We use the if block below to handle the dropdown box selection. 
   - We use tabs to generate anomalous and optimal specs.
   - We display a spinner while the backing function is called, then write the output to the web page.

:::code{showCopyAction=true showLineNumbers=false language=python}
def generate_conditions(model):
  conditions = ['anomalous', 'optimal']
  tabs = st.tabs(conditions)
  for i in range(0, len(conditions)):
    with tabs[i]:
      code_text = 'Based on the motor specs that you provided, please write a JSON message with motor random SENSOR DATA for an ' + conditions[i] + '.\n'
      code = rcalib.call_bedrock(model, code_text)
      st.session_state.generated_code = code
      st.code(code,language=str(conditions[i]))

def display_motor_summary(model):
  st.markdown('**Motor Specs:** \n')
  st.write(str(st.session_state.motor_summary).replace("$","\$"))
  generate_conditions(model)
  
def display_prompt_snesor_data():
  st.markdown("---")  
  st.subheader("**Analyze IoT Sensor Data**\n")
  return st.text_area('**Paste a motor sensor JSON message here, or use one from the examples above.**', key='text')

def describe_sensor_data(model, input_text):
  if(not(input_text and input_text.strip())): return
  
  rca_prompt = "Please write a paragraph describing the following sensors data:\n" + input_text + "\n"
  rca = rcalib.call_bedrock(model, rca_prompt)

  st.subheader('**Interpretation of the IoT sensor data**\n')
  st.write(rca)
  return rca

def run_root_cause_analysis(model, rca):
    if rca is None: return
  
    rca_prompt = "Based on your observations:\n" + rca + "\n and based the motor specs below, TASK: please describe if there is any anomaly and the potential root cause(s):\n ** motor specs **" + st.session_state['motor_summary'] + "\n"
    rca = rcalib.call_bedrock(model, rca_prompt)

    st.subheader('**Root Cause Analysis**\n')
    st.write(rca)

def process_specs(model):
  if st.session_state.motor_summary and len(st.session_state.motor_summary) > 5: 
    display_motor_summary(selected_model)
    sensor_data = display_prompt_snesor_data()
    rca = describe_sensor_data(selected_model, sensor_data)
    run_root_cause_analysis(selected_model, rca)
:::

7. Calling layout and processing functions.

:::code{showCopyAction=true showLineNumbers=false language=python}
initialize_session_state()
configure_page()
add_header()  
st.markdown("---")
add_description()
st.markdown("---")

selected_model = configure_sidebar().lower()
selected_file = select_motor()

upload_file(selected_file, selected_model)
process_specs(selected_model)
:::

8. Save the file.

Outstanding! Now you are ready to run the application!

---
# Run the Streamlit app
1. Select the bash terminal in AWS Cloud9 and change directory.

:::code{showCopyAction=true showLineNumbers=false language=bash}
cd ~/environment/workshop/labs/root-cause-analysis
:::

::::alert{header="Just want to run the app?"}
::::expand{header="Expand here & run this command instead" defaultExpanded=false}
```javascript
cd ~/environment/workshop/completed/root-cause-analysis
```
You can now proceed with step 2 below.
::::

2. Run the streamlit command from the terminal.

:::code{showCopyAction=true showLineNumbers=false language=bash}
streamlit run rca_app.py --server.port 8080
:::
Ignore the Network URL and External URL links displayed by the Streamlit command. Instead, we will use AWS Cloud9's preview feature.

3. In AWS Cloud9, select Preview -> Preview Running Application.

![root cause analysis](/static/root-cause-analysis/cloud9-preview.png)

You should see a web page like below:

![root cause analysis](/static/root-cause-analysis/root-cause-analysis_options.png)

4. Try out sample sensor data and see the results.
   
5. Close the preview tab in AWS Cloud9. Return to the terminal and press Control-C to exit the application.

::::alert{type="success" header="Congratulations!"}
ou have successfully built a root cause analysis with Bedrock, Textract, and Streamlit!
::::
