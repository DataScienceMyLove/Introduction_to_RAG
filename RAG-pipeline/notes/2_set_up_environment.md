# Environment

#### Note: Codespaces: the local advantages in remote

Whether it needs to be local or remote, you have free options.

You can begin your project on Codespaces( VSCode remote environment offered by GitHub) then clone your repository and continues in local.

But you can also use Codespaces with VSCode desktop and have all your VSCode setup like if you develop in local.

**Tutorial:**

- Create a Codespaces in the `Code` button of your repository welcome page
- Wait for the Codespaces opening in your browser
- Click on the blue left bottom bar zone > Open VSCode Desktop
  
You will benefit of Codespaces in local with your extensions if you've sync your settings(left bottom too in Settings)

Codespaces offers a uniformized remote environment. You have a Debian machine, with Python, Docker, git, pip: you can install anything you want:
- install uv
- create virtual environments
- install jupyter to access notebooks
- use bash to access apt repository or automate with bash/sh scripts
- create devcontainers to set up a global environment

Besides VSCode is more practical than the browser version of Codespaces if you are accustomed to the local development.

## 2.1 Prerequisites 

You need the following:

- Python (3.12 or later)
- An [OpenAI account](https://openai.com/) (or an OpenAI-compatible provider like Groq, Gemini, or Ollama)
- Comfortable with Python(how to write control flows, notebooks keyboard shortcuts, loops, write classes, OOP basics)
- confident with the command line

## 2.2 Project set up

After having created your repository with your ReadME, create a directory for your RAG pipeline:

```bash
mkdir RAG-pipeline
cd RAG-pipeline
```

I advice to WIndows users to learn a minimum Bash, all the virtual machines and distant servers are on Linux, it will facilitate you a lot of pain.

Then download **uv**:

Two ways to do that:

1) Install from the astral website, the creators of the software:
   
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

***`curl`*** is almost the same than  ***`wget`***.
It makes HTTP requests and it allows to get the data of a uri or runs a program which installs a software on your machine. Here we pipe the command in sh shell for more portability: bash not being the norm on all servers.

Note: That's better to use this method when you're on your local machine because there seems to me that updates are easier by running uv command.

2) Use the pip command of your remote environment:

```
pip install uv
```
It works, uv is on PyPi.

**Overview on uv:** It's a package manager for Python & it's fast & convenient.

You can:
- create virtual environments like the venv module of the standard library.
- manage Python versions of your machine, even if it's created with pyenv precompiled python versions
- manage temporary environments
- create an initialiazed project structure for a personal project or a package you write for PyPi repo
- choose what dependencies have to be added to the project in development, production or as a tool
- synchronized an environment everywhere it's installed thanks to lock file

It's updated with the new standards of the industry which takes into consideration PEP621 with a pyproject.toml file(like poetry).
That's a kind of a big mixture of all package managers tools since many years with the Rust speed of light.

Initialize a uv project:

```
uv init
```
It doesn't create an environment yet but it initializes a uv project with setup files like:
- `pyproject.toml`: lists the depencies with versions and hashes
- `uv.lock` : the list of packghes for reproductible environments and portability
- `python-version`: writes the used python version of your project
- 
then it depends how uv is set up but it can generate README,.git file, main.py as an entrypoint of your project.

For better explanations: https://astral.sh/

They are also the creators of **Ruff**, one of the most famous linter.

Now add the dependencies we'll need:

```bash
uv add requests minsearch openai jupyter python-dotenv
```
Note that this command will create a virtual environment .venv without calling the venv command 
the Python version pinned in your uv by default will be chosen for your new environment.

Installed packages for project:

| Package | Purpose |
|-|-|
| ***requests*** | to fetch the FAQ dataset from the internet |
| ***minsearch*** | a simple in-memory search engine for indexing and searching text |
| ***openai*** |  the OpenAI API client for calling the LLM |
| ***jupyter*** |  the notebook environment where we'll write and run code |
| ***python-dotenv*** |  to load API keys from a .env file |

## Set up your API keys from your LLM provider

You can choose Anthropic, Gemini but we choose OpenAI models for LLM provider.

- Create an account on : https://platform.openai.com/ and go on the left sidebar menu and select 'API keys'(before that you have to add a minimum of credits, something like 5$)
- Create a separate project in top left button to see the amount of tokens comsumed by this project, it will be separated of your other APIs calls for other projects 
- Create an API key for this project
- Copy your API key

Go to your terminal or in your 'RAG-pipeline' directory space in your vscode interface and create a .env file:
```
touch .env
```
`.env` is a file to pass environment variables to a process in your machine: you list all the environment variables you want to set a separation between the code and the environment behaviour.

It's also the safiest way to store the key that never to be committed to save your credits from evil LLM enjoyers.

That is the reason you have to create a .gitignore and immediately place into your .env file:
```
touch .gitignore
```
In your .env, add:
```
.env
.venv #The virtual environment is useless you have 'uv sync' to synchronize envs 
```
Put your API key in your .env file like this without space before '=':

```bash
OPENAI_API_KEY=sk-YOUR_KEY_HERE
```
 Recall: Never commit your API key, treat it like a password, don't lose your money

 ## Starting Jupyter

Start Jupyter:

```bash
uv run jupyter notebook
```
Note: `uv run` command runs the package in your isolated virtual environment in your .venv created by uv.
To run a command in this environment, whether Python or other package/software: it will use the site-packages or the Python of the environment

Create a new notebook. Throughout the course, you'll copy code from
the section notes into notebook cells.

Check that the OpenAI client works:

```python
from dotenv import load_dotenv
load_dotenv()

from openai import OpenAI
openai_client = OpenAI()
```
- It can take a few seconds to initialize the OpenAI client.
- If `load_dotenv()` loads correctly your environment variable, it will return 'True' in your notebook.
- If you see an error, verify that your .env is saved or there isn't spaces between '=' and your api key string.


Note: Here the python-dotenv package allows us to inject the API key environment variable without introducing it in the parameters of the OpenAI object in the notebook.
OpenAI() is going to detect the environment variable `OPENAI_API_KEY` in our `.env` file and inject it in the process.

Note: `OpenAI()` is the main class of Python SDK from OPENAI. It represents an API client: an object that can communicate with openai servers.
With this object, we can access to some attributes of other subclasses which can also be accessed with the OPENAI API.

When you call this constructor, it's going to be:
- get back the API key to OpenAI servers via environment variable if you don't provide it explicitely in objrct parameters 
- configure a intern HTTP client
- memorize parameters like timeout and base URL of the user
- prepare the different subservices (responses files)

## Set up your API keys from Groq LLM provider

For Groq or other OpenAI-compatible providers, add the key to
`.env`:

```bash
GROQ_API_KEY=your_key_here
```

And configure the client:

```python
from openai import OpenAI
import os

openai_client = OpenAI(
    api_key=os.getenv("GROQ_API_KEY"),
    base_url="https://api.groq.com/openai/v1"
)
```
Note: Groq is compatible with OpenAI Python SDK , you don't use OpenAI servers you just use the Python SDK of OpenAI as generic HTTP client & you redirect the request to Groq API.

## (Optional) Auto-loading .env with dirdotenv

If you don't want to call `load_dotenv()` in every notebook, use
[dirdotenv](https://github.com/alexeygrigorev/dirdotenv).

That's a package maintained by the founder of DatatalksClub, Alexey Grigorev.It's an alternative to python-dotenv package

It loads `.env` files automatically when you `cd` into a directory:

```bash
uv tool install dirdotenv
echo 'eval "$(dirdotenv hook bash)"' >> ~/.bashrc
```

Restart your terminal, and now whenever you enter the project
directory, the variables from `.env` are loaded automatically. No
`load_dotenv()` needed.

Note: To reload this in your vscode environment, save your unsaved files, close your vscode , open a terminal on your local machine:
```bash
cd path/to/your/project/RAG-pipeline
code .
```
Your environment variables of your .env file are injected in the kernel of your notebooks

Note: `uv tool` is a new way to install CLI python tools without polluting your project's environment , it installs packages as global tool in an uv environment separated of your venv.

Packages that are installed from this way won't be synchronized by a 'uv sync' in a new loaded environment.

Note: `eval` is a native Bash command which executes a string as a bash command , that's practical when we want to configure  our shell in modifying our .bashrc file for example.
Note: A 'hook' is an automatic plugin triggered by an event: it's called by the system when:
- a 'cd' command is detected by example
- we're opening a shell
- other system event to activate the effects of a program
  
