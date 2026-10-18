# Hands-On Machine Learning with Scikit-Learn and PyTorch

The goal of this project is to teach you the fundamentals of Machine Learning in Python. It contains the example code and solutions to the exercises in the first edition of my new O'Reilly book [Hands-on Machine Learning with Scikit-Learn and PyTorch (1st edition)](https://homl.info/):

<a href="https://homl.info/er"><img src="https://www.oreilly.com/covers/urn:orm:book:9798341607972/400w/" title="book" width="150" border="0" /></a>

**Note**: If you are looking for the notebooks for the TensorFlow/Keras version of this book, check out [ageron/handson-ml3](https://github.com/ageron/handson-ml3).

> **Polars fork:** this `polars_integration` branch is the community-maintained Polars port of [ageron/handson-mlp](https://github.com/ageron/handson-mlp), maintained at [PGupta-Git/handson-mlp](https://github.com/PGupta-Git/handson-mlp/tree/polars_integration). It tracks the upstream notebooks while keeping the dataframe code current with Polars 2.x and compatible versions of the surrounding ML/visualization stack. An upstream README link to this fork is proposed in [ageron/handson-mlp#41](https://github.com/ageron/handson-mlp/pull/41), following the discussion in [issue #23](https://github.com/ageron/handson-mlp/issues/23).

## Quick Start

### Want to play with these notebooks online without having to install anything?

- <a href="https://colab.research.google.com/github/PGupta-Git/handson-mlp/blob/polars_integration/" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open Polars fork in Colab"/></a> (recommended)

⚠ _Colab provides a temporary environment: anything you do will be deleted after a while, so make sure you download any data you care about._

### Just want to quickly look at some notebooks, without executing any code?

- <a href="https://nbviewer.jupyter.org/github/PGupta-Git/handson-mlp/blob/polars_integration/index.ipynb"><img src="https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg" alt="Render Polars fork in nbviewer" /></a>

- [github.com's notebook viewer](https://github.com/PGupta-Git/handson-mlp/blob/polars_integration/index.ipynb) also works but it's not ideal: it's slower, the math equations are not always displayed correctly, and large notebooks often fail to open.

### Want to install this project on your own machine?

Check out the [installation instructions](INSTALL.md).

The base locked environment is enough for the core notebooks. Some hardware-specific examples use optional extras; for example, Appendix B's 4-bit quantization section needs the `quantization` extra on a supported GPU. See `INSTALL.md` for the exact `uv sync` commands and secret-handling notes.


# FAQ

**Which Python version should I use?**

Python 3.12 is recommended. The project metadata currently supports Python 3.11 through 3.13 (`>=3.11,<3.14`); Python 3.10 is no longer supported by this environment.

**I'm getting an error when I call `load_housing_data()`**

If you're getting an HTTP error, make sure you're running the exact same code as in the notebook (copy/paste it if needed). If the problem persists, please check your network configuration. If it's an SSL error, see the next question.

**I'm getting an SSL error on MacOSX**

You probably need to install the SSL certificates (see this [StackOverflow question](https://stackoverflow.com/questions/27835619/urllib-and-ssl-certificate-verify-failed-error)). If you downloaded Python from the official website, then run `/Applications/Python\ 3.12/Install\ Certificates.command` in a terminal (change `3.12` to whatever version you installed). If you installed Python using MacPorts, run `sudo port install curl-ca-bundle` in a terminal.

**I've installed this project locally. How do I update it to the latest version?**

See [INSTALL.md](INSTALL.md)

**How do I update my Python libraries to the latest versions, when using Anaconda?**

See [INSTALL.md](INSTALL.md)

## Polars compatibility

This fork targets Polars 2.x. The environment requires scikit-learn 1.9.1 or newer because current scikit-learn uses Narwhals for Polars dataframe interoperability after Polars removed the dataframe interchange protocol. Plotting code passes Polars `Series` directly to Matplotlib and to Seaborn's vector-oriented APIs instead of round-tripping through pandas.

The intentionally retained `tools_pandas.ipynb` notebook remains a pandas reference; the Polars port and examples live in the chapter notebooks and `tools_polars.ipynb`.

## Contributors

For changes specific to the Polars port, use [PGupta-Git/handson-mlp](https://github.com/PGupta-Git/handson-mlp). The original project and its contributor history remain at [ageron/handson-mlp](https://github.com/ageron/handson-mlp), and the upstream link proposal for this fork is [PR #41](https://github.com/ageron/handson-mlp/pull/41).
