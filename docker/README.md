# Hands-On Machine Learning in Docker

These Docker instructions also apply to the maintained [Polars fork](https://github.com/PGupta-Git/handson-mlp/tree/polars_integration) of [ageron/handson-mlp](https://github.com/ageron/handson-mlp). For the Polars fork, build the image from the local `polars_integration` checkout so the container uses the fork's Polars/scikit-learn lockfile. The upstream README-link proposal is [PR #41](https://github.com/ageron/handson-mlp/pull/41).

If you would like to run and tweak the book's notebooks in a Docker container, you're in the right place!

## Prerequisites

You must first [Install Docker](https://docs.docker.com/engine/installation/).

For Docker Desktop on Linux, start Docker Desktop before building. If your active context points to another engine, use `docker --context desktop-linux` in place of `docker` in the commands below.

If you have an Nvidia GPU and you're on Linux (or [WSL on Windows](https://learn.microsoft.com/en-us/windows/wsl/install)), then you can use your GPU inside the Docker container. For this, you must ensure that you have the [latest drivers for your GPU](https://www.nvidia.com/drivers/), and install the [Nvidia Container Toolkit](https://github.com/NVIDIA/nvidia-container-toolkit) so Docker can access the GPU.

Below, I will assume that your Docker setup is functional and supports your GPU (if any).

## Usage

The `ageron/handson-mlp` images on Docker Hub follow the upstream repository. They are useful for the upstream notebooks, but they are not guaranteed to contain this fork's Polars-specific dependency set. For the Polars branch, build your own image from the local checkout. The standard image uses the base locked dependencies; hardware-specific optional extras such as Appendix B's `quantization` dependencies are not required for the core image and should only be added when the target platform supports them.

### Download an existing image

Choose the appropriate image, depending on your platform.

```bash
# CPU only
docker pull ageron/handson-mlp:cpu

# Nvidia GPU with CUDA 12.6 compute platform
docker pull ageron/handson-mlp:cu126
```

### Build your own image

Assuming you have already downloaded the project locally in your home folder:

```bash
cd  # go to your home folder
cd handson-mlp
docker build -t handson-mlp-polars:cpu -f docker/Dockerfile --build-arg PT_VARIANT=cpu .
```

The build uses the checkout's `.python-version` and frozen lockfile without adding or removing project dependencies.

The build context allowlist includes `.python-version` but excludes notebooks, host environments, and local secret files. On Linux/amd64, the locked PyTorch 2.8.0 wheel includes the CUDA 12.8 runtime even in the `cpu` image. This image runs on CPU without a GPU, but it is not a CPU-only wheel set or a GPU execution test.

Box2D is optional. To include Chapter 19's Box2D exercises, build with the declared `box2d` extra:

```bash
docker build -t handson-mlp-polars:cpu-box2d -f docker/Dockerfile --build-arg PT_VARIANT=cpu --build-arg WITH_BOX2D=true .
```

The locked Box2D release is wheel-only. If your target platform has no compatible wheel, leave `WITH_BOX2D` disabled or select a supported Python/platform combination. The core notebooks do not require this extra.

If you want to build the Nvidia CUDA 12.6 variant, replace `cpu` with `cu126`. The CUDA installation retains the PyTorch compatibility bounds declared in `pyproject.toml`; keep the Dockerfile and local GPU instructions aligned with those bounds when updating the fork. CUDA builds replace the lockfile's PyTorch family with compatible backend-specific wheels. In the commands below, I'll use `cpu`, but you can replace it with the appropriate variant for your hardware.

> NOTE: You can see the list of available variants by visiting [PyTorch's download page](https://pytorch.org/get-started/locally/) and selecting Linux (even if you are on Windows or MacOS).

This will take quite a while and download gigabytes of data, but it is only required once.

After the process is finished you have a `handson-mlp-polars:cpu` image (or the corresponding GPU variant): it will be the base for your experiments. You can confirm that by running the following command:

```bash
docker images
```

This should output something like this:

```
REPOSITORY            TAG         IMAGE ID            CREATED             SIZE
handson-mlp-polars    cpu         3ebafebc604a        2 minutes ago       2.09GB
```

### Run the notebooks

Run the following commands to start the Jupyter server inside the container (which is named `homlp`):

```bash
cd  # go to the folder containing the handson-mlp directory
cd handson-mlp
docker run -it --rm --name homlp -p 8888:8888 -v "$PWD":/content handson-mlp-polars:cpu
```

To use the optional Box2D image, replace `handson-mlp-polars:cpu` with `handson-mlp-polars:cpu-box2d` in the run command.

If you are using an Nvidia GPU, you must add `--gpus all` to the list of options.

Next, just point your browser to the URL printed on the screen (e.g., `http://127.0.0.1:8888/lab?token=ecaf97203325417dfe17e0824cb80e34cafcaa52e74485b0`), and you're ready to play with the book's code!

The project directory (in your home folder) is mapped to the `/content` directory inside the docker image. This means that the changes you make to the notebooks through the browser are saved in your project directory.

You can close the server by pressing `Ctrl-C` in the terminal window.

This will remove the container so you can start a new one later (but it will not remove the image or the notebooks, don't worry!).

### Verified configuration and limits

On October 24, 2026, both images were built on Linux/amd64 using Docker Engine 29.8.2 through Docker Desktop. The frozen environment selected Python 3.12.15, Polars 2.0.0, scikit-learn 1.9.1, PyTorch 2.8.0, JupyterLab 4.5.0, and Jupyter Server 2.17.0.

The checks exercised:

* Polars expression-based features passed to scikit-learn for fitting and held-out predictions, plus a PyTorch CPU tensor calculation, with Python warnings treated as errors;
* authenticated JupyterLab HTTP startup for both images, followed by a real notebook kernel executing a Polars/scikit-learn prediction through Chrome in the base image;
* the optional image's Gymnasium 1.2.2 / Box2D 2.3.10 `LunarLander-v3` reset and step, with dependency warnings visible; and
* unchanged project, lockfile, and Python-version inputs inside the base image.

These checks do not verify GPU execution or every notebook's training workload. Box2D still has the [strict-warning native-import limitation](../INSTALL.md#optional-softwares-and-libraries), and Pygame reports a deprecated `pkg_resources` import during the optional exercise. No warning suppression or substitute Box2D provider is included.

Have fun!

