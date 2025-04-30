
# Quarto Docs

## Prerequisites

### Quarto

We need to install Quarto onto your local machine. You can [download](https://quarto.org/docs/get-started/), or [install via homebrew](https://formulae.brew.sh/cask/quarto) (if you like that kind of thing):

```sh
brew install --cask quarto
```

If you use VS Code, you can also consider installing the [Quarto Extension](https://marketplace.visualstudio.com/items?itemName=quarto.quarto).

### Setup

Fork the repo. Clone your copy of the repo onto your computer and navigate to it from the command line.

Setup virtual environment (if you like that kind of thing):

```sh
conda create -n quarto-env python=3.10
conda activate quarto-env
```

Install package dependencies:

```sh
#pip install -r requirements.txt
pip install -r docs/requirements.txt
```


## Customization

We are using environment variables to customize video URLs across different copies of the repository.

To set your own video URLs for local development, create a ".env" file in the "docs" folder, and place contents inside like the following:

```sh
# this is the docs/.env" file...

EXERCISE_1_VIDEO_URL="https://vimeo.com/1078779616"
EXERCISE_2_VIDEO_URL="https://vimeo.com/1078782830"
EXERCISE_3_VIDEO_URL="https://vimeo.com/1078783956"
EXERCISE_4_VIDEO_URL="https://vimeo.com/1078785045"
```

## Initialization

FYI the following command was used to initialize the quarto config:

```sh
#quarto create book
```

In the docs/_quarto.yml file, the output directory was changed from _book to _build to standardize across projects, enabling the usage of common workflow files and makefile.


## Building


Previewing the site (runs like a local webserver):

```sh
quarto preview docs/
```


Rendering the site (writes local HTML files to the "docs/_build" directory, which is ignored from version control):

```sh
quarto render docs/ --verbose
```


## GitHub Actions Workflows

### Website Publishing

We are using the ["deploy.yml" workflow configuration file](/.github/workflows/deploy.yml) to deploy the site to GitHub Pages when new commits are pushed to the main branch.

In order for this to work, you first need to configure your GitHub Pages repo settings to publish via GitHub Actions.

If you would like to customize environment variables, you will also need to set them as repository secrets using the repository settings. The GitHub Actions workflow file will read repository secrets and pass them to the build as environment variables (no ".env" file necessary in production).
