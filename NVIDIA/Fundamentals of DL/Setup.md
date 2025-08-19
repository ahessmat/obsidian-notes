1. Starting this by getting my Windows machine setup with miniconda:

https://www.anaconda.com/docs/getting-started/miniconda/install#windows-installation

2. After getting installed, we open the `Anaconda` prompt and configure a virtual environment that supports `PyTorch`. Per their docs as of 18 Aug:

> Currently, PyTorch on Windows only supports Python 3.9-3.13; Python 2.x is not supported.

```powershell
conda create -n dl python=3.13
```

Now we can activate the environment in the future with

```powershell
conda activate dl
```

and then later deactivate it with

```powershell
conda deactivate
```

Our machine 