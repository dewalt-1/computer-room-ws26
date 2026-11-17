---
tags:
  - microdosingai
  - workshop
  - gist
  - V2
location: V2
date: 2026-02-18
---
# Install Ollama

download Ollama from here:
https://ollama.com/download/mac

Or, on a Mac, type this in terminal:
``` bash
curl -fsSL https://ollama.com/install.sh | sh
```

on a Windows machine, type this in PowerShell:
``` powershell
irm https://ollama.com/install.ps1 | iex
```

Now, lets run a model. Open Terminal or PowerShell and type:
``` bash
ollama run tinyllama
```

Ollama will now download and run Mistral, an open source LLM, locally on your machine. This is a basic chat style application to test LLMs. Why Ollama? It is an extremely basic inference engine for LLMs that offers us basic options for inference and research. We're going to use a little known function to alter the LLMs system prompt in order to confuse and break the locally hosted LLM we will work with.

------
## Attention and Tokenization
While the model is being downloaded, lets look at how LLMs interpret language. We're not going to deep dive into LLMs in this workshop, but the most important elements we will work with is attention and tokenization.

### Attention
attention describes the ability of a model to pay attention to the important parts of a sentence (or image, or any other sequential input). It does this by assigning weights to input features based on their importance and their position in the sequence
![[Screenshot 2026-02-17 at 12.29.27.png]]

### Tokenization
Tokenization is the foundational process that allows LLMs to break down human language into digestible pieces called tokens. This transformation is essential as it translates complex language into structured units that the model can interpret, enabling it to “understand” and respond to various linguistic inputs

For example:

> _“Tokenization helps models understand language.”_

When tokenizing this sentence, we break it into smaller pieces called tokens. These tokens could be individual words, subwords, or characters, depending on the tokenization strategy.

- **Word-level tokens** might split this sentence into: `["Tokenization", "helps", "models", "understand", "language"]`.
- **Subword-level tokens** might break it down as: `["Token", "ization", "helps", "model", "s", "understand", "language"]`.
- **Character-level tokens** would split it further: `["T", "o", "k", "e", "n", "i", "z", "a", "t", "i", "o", "n", " ", "h", "e", "l", "p", "s", " ", "m", "o", "d", "e", "l", "s", " ", "u", "n", "d", "e", "r", "s", "t", "a", "n", "d", " ", "l", "a", "n", "g", "u", "a", "g", "e"]`.

### Inference
When you prompt an LLM, the model processes your query token by token, predicting the next likely word or phrase in a sequence based on patterns it learned during training. Unlike training, which is a one-time, resource-intensive process, inference happens repeatedly, often in real-time, as users interact with the model.

![[Pasted image 20260217131830.png]]

----
## Basic inference
lets try out our new model, in terminal, start a conversation with Mistral. In your terminal ,type in a question.

you can exit the conversation and return to your terminal by typing
``` bash
/exit
```

# Breaking LLMs
in this workshop, we'll use a method to past a system prompt on top of our downloaded model, in order to change the way that model behaves. 

## System prompt
Your prompting of the LLM is what is know as the "user prompt". The instruction a user gives an LLM to trigger inference. A system prompt is a set of rules and instructions that determines how the user prompt is interpreted and answered.

To break or LLM, we will create diverse system prompts to make the LLM interpret our prompts in weird, unexpected ways. 

To make this system prompt in Ollama, we'll create a modfile, and bake it unto our LLM (in this case, Mistral)
# Make a modefile 
Create a working folder somewhere in your machine and cd into it. Then, in terminal, type:

```bash
touch your_model_name
nano your_model_name
```

This will open nano, a basic text editor in our terminal.

in nano:
```bash
#sets the model to work with
FROM Mistral

# sets a custom system message to specify the behavior of the model
SYSTEM You are Mario from super mario bros, acting as an assistant.
```
Control + X, Y to save and then enter

then, in terminal:
``` bash
ollama create your_model_name -f location of the file e.g. ./your_model_name
ollama run your_model_name
```

Now, Ollama will create a new model, based on our new system prompt.
\
## Going further
now, lets add Parameters, these help us changethe model inference

``` bash
PARAMETER <parameter> <parametervalue>
```

 For example:
```bash
FROM Mistral
# sets the temperature to 1 [higher is more creative, lower is more coherent]
PARAMETER temperature 1.0 
# sets the context window size to 4096, this controls how many tokens the LLM can use as context to generate the next token
PARAMETER num_ctx 4096

# sets a custom system message to specify the behavior of the chat assistant
SYSTEM You are Mario from super mario bros, acting as an assistant.
```
Control + X, Y to save and then enter

In this workshop, we will work with these parameters (although there are a lot more):

| Parameter   | Description                                                                                                                                                                             | Value Type | Example Usage   |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- |
| num_ctx     | Sets the size of the context window used to generate the next token. (Default: 2048)                                                                                                    | int        | num_ctx 4096    |
| temperature | The temperature of the model. Increasing the temperature will make the model answer more creatively. (Default: 0.8)                                                                     | float      | temperature 0.7 |
| top_k       | Reduces the probability of generating nonsense. A higher value (e.g. 100) will give more diverse answers, while a lower value (e.g. 10) will be more conservative. (Default: 40)        | int        | top_k 40        |
| top_p       | Works together with top-k. A higher value (e.g., 0.95) will lead to more diverse text, while a lower value (e.g., 0.5) will generate more focused and conservative text. (Default: 0.9) | float      | top_p 0.9       |

# install ollama on RPI
interminal type:
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

On a Rpi, small models like tinyllama or SmoLLM work better with the limited hardware