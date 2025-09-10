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

Our windows machine doesn't have CUDA installed, which we determined by running 

```
nvcc --version
```

See [this reference](https://github.com/bycloudai/SwapCudaVersionWindows), which speaks to swapping CUDA versions on Windows. We'll use an older version of CUDA at 12.9 (which aligns to pytorch).


Next - with the `dl` virtual environment configured:

```powershell
conda install -y jupyter jupyterlab notebook ipykernel 
```

```powershell
conda install -y -c conda-forge numpy scipy pandas scikit-learn matplotlib seaborn transformers datasets tokenizers accelerate evaluate optimum huggingface_hub nltk category_encoders
```

```powershell
conda install -c conda-forge xgboost
```

And then, finally, we get pytorch installed:

```python
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu129
```
