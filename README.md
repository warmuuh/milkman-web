# Milkman web

source code for [https://milkman.dev](https://milkman.dev)

## checkout

```bash
git submodule init
git submodule update
```

## How to build
this project uses [hugo](https://gohugo.io/) as static site generator. To build the project, you need to have hugo installed.

```bash
brew install hugo

hugo
## for testing locally:
hugo server --navigateToChanged
```

## How to deploy
the project uses firebase for deployment. To deploy the project, you need to have firebase-tools installed.

```bash
brew install firebase-cli
firebase login
firebase deploy
```
