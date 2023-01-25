# Thesis


<!-- Replace the urls here to get a badge of build status. You can find yours at [your-repo-url]/actions/workflows/build-latex.yml > "..." > "Create status badge". (Or just replace the repo refrence in the URLs below) -->

[![Build LaTeX document](https://github.com/rosvik/thesis/workflows/Build%20LaTeX%20document/badge.svg)](https://github.com/rosvik/thesis/actions/workflows/build-latex.yml)


## PDF download

<!-- Update the urls here to get direct links to downloads. The URL will point to the latest release _not_ marked as pre-release. -->

Latest version: [Download](https://github.com/rosvik/thesis/releases/latest/download/main.pdf)

Older versions can be found under [Releases](https://github.com/rosvik/thesis/releases/).

## GitHub setup

To make your own thesis repo, create one by clicking "Use this template" -> "Create a new repository" above, and give it a fitting name. Clone it to your machine, and continue to set up LaTeX and a local writing environment in VS Code with the instructions below.

To make the GitHub actions build system work, you need to give write access to your repo. Do this by going to "Settings" -> "Actions" -> "General", and enable "Read and write permissions" under "Workflow permissions". After pushing changes or manually running the action, you should see a pre-release show up under "Releases".

## Local environment setup

Installing LaTeX locally isn't the most straight forward thing in the world, but this will hopefully guide you trough most of it. However, this is the recipie for macOS only, so you will probably need to adjust a bit if you're on another OS.

We'll look at setting up TeX Live, a bunch of packages, latexindent and finally some configuration of VS Code.

### Setting up TeX Live

Using [TeX Live](https://tug.org/texlive/quickinstall.html)/[tlmgr](https://www.tug.org/texlive/tlmgr.html) (TeX Live package manager) and [ctan.org](https://www.ctan.org/) to install packages.

Installing `tlmgr` with a basic set of default packages (~300MB):

1. Download installer from https://tug.org/texlive/acquire-netinstall.html
2. Run `install-tl`
3. Enter command `S` - set installation scheme
4. `d` - basic scheme
5. `R` - return to main menu
6. `I` - start installation to hard disk

After installation, look at the installation output, and add the mentioned paths to your `PATH`, `MANPATH` and `INFOPATH` environment variables. E.g. using zsh, add this to `~/.zshrc`:

```
export PATH="/usr/local/texlive/2022/bin/universal-darwin:$PATH"
export MANPATH="/usr/local/texlive/2022/texmf-dist/doc/man:$MANPATH"
export INFOPATH="/usr/local/texlive/2022/texmf-dist/doc/info:$INFOPATH"
```

After a restart of your terminal, the `tlmgr` command should be available.

Finally, set the package repository, and check for updates to packages (may need sudo):

```
tlmgr option repository ctan
tlmgr option repository https://mirror.ctan.org/systems/texlive/tlnet
tlmgr update --all
```

### Installing TeX packages

<!-- ```
tlmgr install acmart iftex xstring environ totpages trimspaces manyfoot \
    ncctools comment balance preprint quoting libertine newtx zi4 inconsolata \
    todonotes chktex latexindent breakcites biblatex enumitem pdfpages
```
not present: zi4, balance, manyfoot -->

```
tlmgr install acmart iftex xstring environ totpages trimspaces \
    ncctools comment preprint quoting libertine newtx inconsolata \
    todonotes chktex latexindent breakcites biblatex enumitem pdfpages \
    microtype setspace booktabs upquote pdflscape
```


### Setting up [latexindent](https://github.com/cmhughes/latexindent.pl)

To enable automatic indentation trough latexindent work, you may need to install the `"File::HomeDir"` perl package.

```sh
sudo perl -MCPAN -e 'install "File::HomeDir"'
```

### VS Code config

My Visual Studio Code configuration for LaTeX and BibTeX is included in [.vscode/settings.json](.vscode/settings.json). Some prerequisites are:

- [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) (VS Code extension with lots of LaTeX features)
- [ChkTeX](https://www.nongnu.org/chktex/) (LaTeX linter)
- [latexindent.pl](https://github.com/cmhughes/latexindent.pl) (Automatic indentation)

The rest of my VS Code configurations are available at [rosvik/dotfiles -> vscode/settings.json](https://github.com/rosvik/dotfiles/blob/master/vscode/settings.json)

## Generate PDF

A PDF is generated automatically on every save in VS Code, as well as on every push through the [`build-latex.yml` GitHub Action](.github/workflows/build-latex.yml). If you want to generate a PDF manually, run:

```
bibtex main.aux && pdflatex -synctex=1 -interaction=nonstopmode \
    -file-line-error -recorder -output-directory=. main.tex
```
