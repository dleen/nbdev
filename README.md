
# Getting started

> Create delightful software with Jupyter Notebooks

<div>

![CI](https://github.com/fastai/nbdev/actions/workflows/test.yaml/badge.svg)

</div>

*NB: This is nbdev v2, a major upgrade of nbdev. Whilst the differences
to nbdev1 aren’t huge, it does require some changes. The old version
docs are at [nbdev1.fast.ai](https://nbdev1.fast.ai). You can use
version-pinning in `settings.ini` (i.e `'nbdev<2'`) to stop nbdev from
upgrading. To upgrade, follow the [migration
tutorial](https://nbdev.fast.ai/migrating.html).*

------------------------------------------------------------------------

`nbdev` is a notebook-driven development platform. Simply write
notebooks with lightweight markup and get high-quality documentation,
tests, continuous integration, and packaging for free!

`nbdev` makes debugging and refactoring your code much easier than in
traditional programming environments since you always have live objects
at your fingertips. `nbdev` also promotes software engineering best
practices because tests and documentation are first class.

- **Documentation** is automatically generated using
  [Quarto](https://quarto.org/) and hosted on [GitHub
  Pages](https://pages.github.com/). Docs support LaTeX, are searchable,
  and are automatically hyperlinked (including out-of-the-box support
  for many packages via
  [`nbdev-index`](https://github.com/fastai/nbdev-index))
- **Publish packages to PyPI and conda** as well as tools to simplify
  package releases. Python best practices are automatically followed,
  for example, only exported objects are included in `__all__`
- **Two-way sync between notebooks and plaintext source code** allowing
  you to use your IDE for code navigation or quick edits
- **Tests** written as ordinary notebook cells are run in parallel with
  a single command
- **Continuous integration** out-of-the-box with [GitHub
  Actions](https://github.com/features/actions) that run your tests and
  rebuild your docs
- **Git-friendly notebooks** with [Jupyter/Git
  hooks](/01_Tutorials/02_git_friendly_jupyter.ipynb) that clean
  unwanted metadata and render merge conflicts in a human-readable
  format
- … and much more!

## Install

nbdev works on macOS, Linux, and most Unix-style operating systems. It
works on Windows under WSL, but not under cmd or Powershell.

You can install nbdev with pip:

``` sh
pip install nbdev
```

… or with conda (or mamba):

``` sh
conda install -c fastai nbdev
```

Note that `nbdev` must be installed into the same Python environment
that you use for both Jupyter and your project.

## How to use nbdev

The best way to learn how to use nbdev is to complete either the
[written
walkthrough](https://nbdev.fast.ai/01_Tutorials/01_tutorial.html) or
video walkthrough:

<div style="text-align: center">

<div>

<a href="http://www.youtube.com/watch?v=l7zS8Ld4_iA" target="_blank"
title="nbdev walkthrough"><img
src="https://github.com/fastai/logos/raw/main/nbdev_walkthrough.png"
style="border-radius: 10px" width="560" height="315" /></a>

</div>

</div>

Alternatively, there’s a [shortened version of the video
walkthrough](https://youtu.be/67FdzLSt4aA) with coding sections sped up
using the `unsilence` Python library – it’s 27 minutes faster, but a bit
harder to follow.

You can also run `nbdev_help` from the terminal to see the full list of
available commands:

``` python
!nbdev_help
```

    nbdev_bump_version              Increment version in settings.ini by one
    nbdev_changelog                 Create a CHANGELOG.md file from closed and labeled GitHub issues
    nbdev_clean                     Clean all notebooks in `fname` to avoid merge conflicts
    nbdev_conda                     Create a `meta.yaml` file ready to be built into a package, and optionally build and upload it
    nbdev_create_config             Create a config file.
    nbdev_deploy                    Deploy docs to GitHub Pages
    nbdev_docs                      Create Quarto docs and README.md
    nbdev_export                    Export notebooks in `path` to Python modules
    nbdev_filter                    A notebook filter for Quarto
    nbdev_fix                       Create working notebook from conflicted notebook `nbname`
    nbdev_help                      Show help for all console scripts
    nbdev_install                   Install Quarto and the current library
    nbdev_install_hooks             Install Jupyter and git hooks to automatically clean, trust, and fix merge conflicts in notebooks
    nbdev_install_quarto            Install latest Quarto on macOS or Linux, prints instructions for Windows
    nbdev_merge                     Git merge driver for notebooks
    nbdev_migrate                   Convert all directives and callouts in `fname` from v1 to v2
    nbdev_new                       Create an nbdev project.
    nbdev_prepare                   Export, test, and clean notebooks, and render README if needed
    nbdev_preview                   Preview docs locally
    nbdev_pypi                      Create and upload Python package to PyPI
    nbdev_quarto                    Create Quarto docs and README.md
    nbdev_readme                    Render README.md from index.ipynb
    nbdev_release_both              Release both conda and PyPI packages
    nbdev_release_gh                Calls `nbdev_changelog`, lets you edit the result, then pushes to git and calls `nbdev_release_git`
    nbdev_release_git               Tag and create a release in GitHub for the current version
    nbdev_sidebar                   Create sidebar.yml
    nbdev_test                      Test in parallel notebooks matching `path`, passing along `flags`
    nbdev_trust                     Trust notebooks matching `fname`
    nbdev_update                    Propagate change in modules matching `fname` to notebooks that created them

## FAQ

### Q: What is the warning “Found a cell containing mix of imports and computations. Please use separate cells”?

A: You should not have cells that are not exported, *and* contain a mix
of `import` statements along with other code. For instance, don’t do
this in a single cell:

``` python
import some_module
some_module.something()
```

Instead, split this into two cells, one which does `import some_module`,
and the other which does `some_module.something()`.

The reason for this is that when we create your documentation website,
we ensure that all of the signatures for functions you document are up
to date, by running the imports, exported cells, and `show_doc`
functions in your notebooks. When you mix imports with other code, that
other code will be run too, which can cause errors (or at least
slowdowns) when creating your website.

### Q: Why is nbdev asking for root access? How do I install Quarto without root access?

A: When you setup your first project, nbdev will attempt to
automatically download and install [Quarto](https://quarto.org/) for
you. This is the program that we use to create your documentation
website.

Quarto’s standard installation process requires root access, and nbdev
will therefore ask for your root password during installation. For most
people, this will work fine and everything will be handled automatically
– if so, you can skip over the rest of this section, which talks about
installing without root access.

If you need to install Quarto without root access on Linux, first `cd`
to wherever you want to store it, then [download
Quarto](https://quarto.org/docs/get-started/), and type:

``` bash
dpkg -x quarto*.deb .
mv opt/quarto ./
rmdir opt
mkdir -p ~/.local/bin
ln -s "$(pwd)"/quarto/bin/quarto ~/.local/bin
```

To use this non-root version of Quarto, you’ll need `~/.local/bin` in
your [`PATH` environment
variable](https://linuxize.com/post/how-to-add-directory-to-path-in-linux/).
(Alternatively, change the `ln -s` step to place the symlink somewhere
else in your path.)

### Q: Someone told me not to use notebooks for “serious” software development!

A: [Watch this video](https://youtu.be/9Q6sLbz37gk). Don’t worry, we
still get this too, despite having used `nbdev` for a wide range of
“very serious” software projects over the last three years, including
[deep learning libraries](https://github.com/fastai/fastai), [API
clients](https://github.com/fastai/ghapi), [Python language
extensions](https://github.com/fastai/fastcore), [terminal user
interfaces](https://github.com/nat/ghtop), and more!

## Contributing

If you want to contribute to `nbdev`, be sure to review the
[contributions
guidelines](https://github.com/fastai/nbdev/blob/master/CONTRIBUTING.md).
This project adheres to fastai’s [code of
conduct](https://github.com/fastai/nbdev/blob/master/CODE_OF_CONDUCT.md).
By participating, you are expected to uphold this code. In general, we
strive to abide by generally accepted best practices in open-source
software development.

Make sure you have `nbdev`’s git hooks installed by running
`nbdev_install_hooks` in the cloned repository.

## Copyright

Copyright © 2019 onward fast.ai, Inc. Licensed under the Apache License,
Version 2.0 (the “License”); you may not use this project’s files except
in compliance with the License. A copy of the License is provided in the
LICENSE file in this repository.
