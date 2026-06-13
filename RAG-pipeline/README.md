# Steps to build the RAG pipeline

## 1) Introduction

In this working directory we're going to build a ***working RAG system***.

### What is RAG?

**R** is for ***Retrieval***, **A** for ***Augmented***, **G** for ***Generation***, said with the acronym ***RAG***.

We're going to retrieve all these parts in the build of the pipeline.

***RAG is not an ensemble that can't be separated*** — all the parts can be changed with another softwares or technologies.

By example the "G part" is powered by the **LLM** and can be provided by *OpenAI* (gpt models), *Anthropic* (Claude models), etc through their APIs.

We will see it later through the advancement of the pipeline, notebooks and this following README.

### 1.1) Third party packages and environment package manager

We'll write everything in **Python** with third-party modules and standard library.

#### We will use packages like:

- **Jupyter** for using notebooks as pipeline & tests of different alternatives of **development** for your code
- **requests** for HTTP requests to fetch the necessary data on FAQ DatatalksCLub website
- **python-dotenv** to inject environment variables (API keys) in a Python process and a notebook
- **openai** the OpenAI Python SDK for APIs requests and get LLM responses to our answers.
- **minsearch** a lightweight search engine for indexing and searching text (spoiler: the base of the R of RAG)

#### To handle the dependencies & the project's structure:

***`uv`*** is the ***best alternative*** of the moment.

- It offers ***project/package structure***
- you can create ***temporary/permanent Python virtual environments***
- ***`pyproject.toml`, lock file, python-version file***: you can synchronize your environment with your dependencies and the Python version of your choice or those of other repos everywhere it's installed

We'll speak about these softwares during the workshop explaining their features brought to the building of our RAG.

### 1.2) Bricks of the project

#### Why can't we take just the **LLM** to answer to our user's question for one course among those DTC's? Why do we need all these ***RAG components***?

We're going to speak about ***LLMs*** a little bit more and their ***limits for local/precise data***.

#### LLM: *Large Language Models*, a great power but doesn't rule everything

A ***LLM*** is a ***neural network*** trained on a ***massive amount of text***. Generally on the big majority of Internet, social medias, books, studies, courses, all can be preprocessed and tokenized is used by the model for training. Like all machine learning models, we can give us an input to receive an output to answer to a real world problem with more and less precision.

Given a prompt, it generates an output based on what he learnt during its training.

So it generates a continuation, a plausible next piece of text based on a ***probability***. ***It's not deterministic*** — you will have a different output for a same prompt based on interaction with it before this...

That's the same concept visible with Whatsapp when you're trying to write a phrase and Whatsapp gives you a proposition for the next word on the most probable word based on what you typed so far... In this case, WH just uses simple language model.

A LLM does the same thing but at ***much larger scale***. It has ***Billions ++ of parameters*** and is trained on most of the text of the Web.

When it predicts the same word it feels like you're talking with a real human being, it looks like to understand what you say, it sounds like perfect reasoning as a specialist about the subject you launch with it (him/her?)

Here we will treat LLM as ***black boxes*** because it's too specialized, we won't look inside or cover the theory, neither train, fine-tune, host a model by ourselves.

We are going to use an LLM provider & call it over an API. We will provide a prompt for a response of the LLM through the API.

#### Limits of the LLM for precise use cases:

We don't know about what it happens into the model but ***we know how it behaves***.

***LLMs have limitations*** concerning:

- ***Knowledge cutoff***: they only know what was in their training data. So if you ask something else that happened after training, they won't know and their biggest default is they don't know to say 'I don't know' when they don't know so they'll make something up.

- ***No access to your local data*** — they can't see your documents databases or internal systems unless you provide them these informations.

- ***Hallucinations***: they sometimes sound like experts but it's poker face. Their answers can be wrong cause of a lack of good data for this subject or a high probability token which doesn't get back the reality. That's what we call **hallucinations** of the model.

#### RAG: *a solution to compensate the LLM limitations*

***LLMs*** have fitted with an enormous amount of data to give answers at high scale but can lose theirselves in this 'too many information' land.

And their ability to interact with the data can be used to have good answers to our questions with a little data injection of our part.

***`RAG` can help the LLM to reach its target like the sight of a weapon.***

By giving to the **LLM** the ***relevant documents*** at question time, we give the ***context*** to interact with it so we retrieve the right information and the model will generate a ***grounded response*** based on *local/precise/real-time information*.

It allows us to inject ***fresh information*** the model never saw during the training. That's why ***RAG can compensate the defaults*** & the omniscient-side of the model can be drived to ***reduce hallucinations*** and ***improve the quality*** of the answer to the prompt.

But how is composed a RAG? It will depend on our problem and what use case we want to resolve. ***LLM*** is a part of the RAG it's the ***Generation*** part of the whole thing.

#### Use case: Provide an ***assistant*** for ***students questions*** about all the ***DatatalksCLub courses***

To make this concrete, we are going to build step by step a ***FAQ assistant*** with RAG empowerment to the beginning then make it becoming a ***FAQ agent*** which can answer to a student who asks any question like:

```
"Hey agent, when does the LLM Zoomcamp start? Can I enroll late? How can i be graduated?"
```




