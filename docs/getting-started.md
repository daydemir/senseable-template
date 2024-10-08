# Getting Started

## Prerequisites

In order to get started, you'll need the following:

1. A basic understanding of **git** and a GitHub account.
2. A command line interface to run commands (often Terminal on macOS and Linux, and Command Prompt or PowerShell on Windows).
3. An installation of **pipx** or **pip**.

## Setup

Below, we walk through how to quickly set up your new project. This README provides two different setup flows for different expertise and preference profiles. If you would prefer, you are welcome to deviate from these setup flows and make choices as you see fit. More details can be found in [the documentation for the cookiecutter-data-science template](https://cookiecutter-data-science.drivendata.org).

### *Standard*
- Allows you to choose between using **pip** & **venv**, or **conda** for dependency and virtual environment management
- If you plan to deploy a package, **Flit** will be the default tool to use.
- This set up will be best for those who want to use the more traditional `requirements.txt` file for dependency management, or for users who want more flexibility for more complex projects.

### *Poetic*
- Basic setup with a few extra steps to start using **Poetry** for all of dependency management, virtual environment generation, and package publication.
- This setup may be better suited for those who plan to only use Python code and packages in their project.
- The downside of this set up is that sharing code with other users takes an extra step if they do not have (and do not want to use) **Poetry** for dependency management.

While **Poetry** can also be used for projects set up with the *Standard* installation as well, the *Poetic* set up best takes advantage of the unification that *Poetry* provides.


### Install `ccds`
The `cookiecutter-data-science` template team created a command line tool called `ccds` to run the setup.

With pipx (recommended)
```shell
pipx install cookiecutter-data-science
```
With pip
```shell
pip install cookiecutter-data-science
```

## Shared Setup
Run `ccds` from the parent directory where you want your project stored
```shell
# From the parent directory where you want your project
ccds
```

Begin filling out the template prompts. These can all be changed later.

- Enter the `project_name`
- Enter the `repo_name`. Choose a name that has not been used for a repository on your GitHub account.
- Enter the `module_name`. If you choose to export your standalone code, this will be the name of the Python package.
- Enter the `author_name`. This can be your full name.
- Enter the `description`.
- Enter the `python_version_number`. As of mid-2024, using a version >=3.9 is recommended.
- Select the `dataset_storage`.
   - For large datasets or CSVs, it is recommended they be stored on a cloud storage if possible. If you have details for those cloud storage providers, go ahead and choose the one you use and provide the credentials.
   - If you do not currently use one of those providers but plan to do so eventually, you can select it now and use the default values for the credentials. This will create the convenient scripts in the `Makefile`, and later when you set up the cloud storage you can modify the details in the `Makefile` to reflect the relevant credentials (bucket name, etc).
   - Eventually, we will recommend using the SCL Network Attached Storage (NAS) that is hosted in Cambridge for data storage. See the section ][Using the SCL Network Attached Storage (NAS)](working-with-your-project.md#using-the-scl-network-attached-storage-nas) below for instructions on how to set up the NAS for data storage. If you will use the SCL NAS, select `none` for `dataset_storage`.

For the remaining choices, choose a setup flow below to follow.

## *Standard* Setup
### Complete template setup

- Select the `environment_manager` of your choice. Select `none` if you wish to set up an environment manager that is not listed.
- Select the `dependency_file` of your choice.
- For `pydata_packages`, select `basic` if you would like the following packages added from the start: ipython, jupyterlab, matplotlib, notebook, numpy, pandas, scikit-learn.
- Select `open_source_license`. `MIT` is recommended if the repository will be public.
- Select `docs`. If you would prefer to use [Read the Docs](https://about.readthedocs.com) instead, you can select `none` and set that up independently.
- Select `code_scaffold`. It is recommended to select `yes` to include some useful boilerplate code. If you are an experienced Python developer, you may prefer to avoid the prebuilt boilerplate and select `no`.

### Create a Python virtual environment
If you used the *Standard* setup, you can use the following command to create a new environment using a recipe provided by the template in the Makefile:
```shell
make create_environment
```
You will then need to activate that environment using the environment manager you've chosen. See [Create a Python virtual environment](https://cookiecutter-data-science.drivendata.org/using-the-template/#create-a-python-virtual-environment) for more information.

You can read more about the Makefile in the [Working On Your Project](working-with-your-project.md#make) section.

## *Poetic* Setup
### Complete template setup
- For `environment_manager`, select `none`.
- For `dependency_file`, select `requirements.txt`. At the end of the setup, we will remove this and use `pyproject.toml` instead.
- For `pydata_packages`, select `none`.
- Select `open_source_license`. `MIT` is recommended if the repository will be public.
- Select `docs`. If you would prefer to use [Read the Docs](https://about.readthedocs.com) instead, you can select `none` and set that up independently.
- Select `code_scaffold`. It is recommended to select `yes` to include some useful boilerplate code. If you are an experienced Python developer, you may prefer to avoid the prebuilt boilerplate and select `no`.

### Set up Poetry
In the *Poetic* setup flow, we will manage dependencies using [Poetry](https://python-poetry.org). With **Poetry**, you only need one tool to cover the following: manage dependencies, create local environments, and publish your package to PyPI.

#### Install Poetry
Simple installation with `pipx`
```shell
pipx install poetry
```
See detailed instructions [here](https://python-poetry.org/docs/#installation).

#### Initialize Poetry
The *cookiecutter-data-science* template created a `pyproject.toml` for us already, but we will want to delete a couple sections from that before initiaalizing **Poetry**.

First, delete the sections within the `pyproject.toml` for `[project]` and `[build-system]`. They will probably look something like this:
```toml
[project]
name = "your-project-name"
version = "0.0.1"
description = "the description for your project"
authors = [
  { name = "Deniz Aydemir" },
]
license = { file = "LICENSE" }
readme = "README.md"
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License"
]
requires-python = "~=3.12"

[build-system]
requires = ["flit_core >=3.2,<4"]
build-backend = "flit_core.buildapi"
```

After removing this text from the `pyproject.toml`, you can now initialize using **Poetry**. From within the newly created project folder run the following:
```shell
cd newly-created-project-folder 
poetry init
```
You will be guided through set up options, and you should chose the same project name you chose during the original set up.

#### Configure in-project virtual environments
```shell
poetry config virtualenvs.in-project true
```
#### Install to download dependencies and setup environment
```shell
poetry install
```
This creates a virtual environment that stored in the project folder at `/.venv`. This folder is hidden to git thanks to the `.gitignore` file.

If someone else wants to run your code, they can clone or download your repository from GitHub, and run `poetry install` themselves to have a copy of the virtual environment you are using.

#### Add included dependencies
Let's add the helpers that come with the template to the development environment. More information on this is available in [Working On Your Project](working-with-your-project.md#dependency-management).
```shell
poetry add black flake8 isort --group dev
```

If you chose `yes` for the `code_scaffolding` option, then run the following command to add the packages included in the boilerplate code to the whole project.

```shell
poetry add python-dotenv loguru typer tqdm
```

#### Delete the requirements.txt

In order to avoid confusion, it is recommended that you delete the `requirements.txt` file as it will not be used.

### pyproject.toml
It will be useful to familiarize yourself with the `pyproject.toml` file. This file is the current best practice method for defining project parameters and dependencies. Poetry will populate and use this file as you add dependencies. Other build tools like Flit, Hatch, and Setuptools also use the `pyproject.toml`. Read more about `pyproject.toml` at [Python's pyproject documentation](https://python-poetry.org/docs/pyproject/).

#### What happened to requirements.txt?
When using Poetry, all your dependencies are managed in the `pyproject.toml` file.
It is possible to use a `requirements.txt` to populate your dependencies in your `pyproject.toml` with Poetry.
```shell
poetry add $(cat requirements.txt)
```
Similarly, it is possible to export your dependencies from the `pyproject.toml` to a `requirements.txt` if needed.
```shell
# Export dependencies to requirements.txt
poetry export -f requirements.txt --output requirements.txt

# If you want to include development dependencies as well
poetry export -f requirements.txt --output requirements.txt --dev
```

#### Adjust Makefile
In fact, in order to get some of our `Makefile` recipes working again, we will add those above lines to the `requirements` recipe. So now the `requirements` recipe in your `Makefile` should look like this:
```make
## Install Python Dependencies
.PHONY: requirements
requirements:
	poetry export -f requirements.txt --output requirements.txt
	poetry export -f requirements.txt --output requirements.txt --dev
	$(PYTHON_INTERPRETER) -m pip install -U pip
	$(PYTHON_INTERPRETER) -m pip install -r requirements.txt
```

It is important to note that with this change the `make requirements` recipe will create a `requirements.txt` based on your **Poetry** dependencies (both regular and development) based on the `pyproject.toml`, and install those dependencies using **pip**. However, if you are using **Poetry** to manage your dependencies, `make requirements` should not be used to install dependencies. Instead, the standard `poetry install` should be used.

The Makefile is discussed further in [Working With Your Project](working-with-your-project.md#make).

## Connect to GitHub
Whichever setup flow you chose, your final step will be to connect your local folder to a new repo on GitHub.

In order to complete this step, ensure you have the [GitHub command-line interface (CLI)](https://cli.github.com) installed. Then you can run the following commands to connect your local repository to GitHub.

```shell
# Navigate to your newly created project folder
cd newly-created-project-folder

git init                        
git add .                       
git commit -m "CCDS defaults"   

# Use GitHub CLI to create a new repo on your account. This will prompt you for further details.
gh repo create
```

Your project is now ready to go!