---
title: "Command-line ChatGPT Python Script Coded by ChatGPT"
description: "Command-line ChatGPT Python Script Coded by ChatGPT"
date: "2022-12-20"
categories: [Lab]
tags: [openai, chatgpt, python]
pin: false
math: true
mermaid: true
image:
  path: "/assets/img/chatgpt/chatgpt.jpg"
  alt: "ChatGPT"
---

ChatGPT is a variant of the popular language model GPT (Generative Pre-trained Transformer) that has been specifically designed for chatbot applications. It is capable of generating human-like responses to text input, making it a useful tool for building chatbots and virtual assistants.

In the field of cyber security, ChatGPT could be used to build a chatbot that helps users identify and mitigate cyber threats. For example, a chatbot built with ChatGPT could be integrated into a company's cybersecurity system and used to provide real-time advice and guidance to employees on how to stay safe online.

In addition to providing advice and guidance, ChatGPT could also be used to simulate cyber attacks in order to test the effectiveness of a company's cybersecurity measures. This could be done by training the model on a wide range of potential attack scenarios and then using it to generate realistic attacks that can be used to test the company's defenses.

Overall, ChatGPT has the potential to be a valuable tool in the fight against cyber threats, helping organizations to stay safe and secure in the digital age.

But for now, we just want to create a simple python script to query within the CLI. However, not just any script. I've  asked ChatGPT to code a python script which can query the desired questions directly using the API. It will have a prompt and when we type "exit", it will terminate the script. 

Like so:

![chatgpt_chat](/assets/img/chatgpt/chatgpt_chat.png)

<br>

Using the openai library to generate a response from ChatGPT

```py
response = openai.Completion.create(
        engine="text-davinci-002",
        prompt=prompt,
        max_tokens=1024,
        temperature=0.7
    )
```

`max_tokens` can be set up to `4097`. If you want longer responses, you can set a higher value.

`temperature` value determines the randomness or creativity of the model's output. It can be set between `0` to `1`. The default value is `0.7`.

Getting the user input and terminating the script when typing in "exit".

```py
    user_input = input("User: ")

    if user_input.lower() == "exit":
        break
```

But as you can see, it's a bit boring. When you ask a question, it answers. That's all.

So, let's try to make it a bit more conversational, meaning ChatGPT's responses will reflect those of the previoiusly asked questions. More like a chat box.

Like so:

![chatgpt_chat2](/assets/img/chatgpt/chatgpt_chat2.png)

### What you will need

1. Create an account for ChatGPT. 
2. Obtain the API Key.
3. install `openai` module.

Once you have the required information, just save the api key to a text file and put the path inside the script.

### Full script

Installing openai module.

`> python3 -m pip install openai`

Update the api key path and save the script.

`> python3 chatGPT.py` 

```python
import openai

# Set the API key for your OpenAI account
openai.api_key_path = "/path/to/openai/key"

# Set the initial prompt for ChatGPT
prompt = input("User: ")

# Loop indefinitely to continue the conversation
while True:
    # Use the openai library to generate a response from ChatGPT
    response = openai.Completion.create(
        engine="text-davinci-002",
        prompt=prompt,
        max_tokens=3800,
        temperature=0.7
    )

    # Print the response
    response_text = response["choices"][0]["text"]
    print(f"Bot: {response_text.strip()}\n")

    # Get the user's input
    user_input = input("User: ")

    # Check if the user's input is "exit" and exit the script if it is
    if user_input.lower() == "exit":
        break

    # Update the prompt with the user's input
    prompt = f"{response_text.strip()}\nUser: {user_input}"

```



To conclude, it's not all butterflies and roses when querying through ChatGPT for the code and there are still human touches required here and there. Nevertheless, it's amazing what ChatGPT can do today and it will only improve better and better overtime. Imagine what it will be able to do in the next two years. 