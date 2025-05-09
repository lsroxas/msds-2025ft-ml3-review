# MSDS 2025 FT ML3 Midterm Review Session

Code and slide deck used in the midterm review session discussion for the ML3 course of MSDS 2025 FT.

## Setup

Locate the `environment.yml` file in your directory then use `mamba` or `conda` to install it in your machine. Use the environment to ensure consistent versions of the libraries are used with the ones in the notebook.

```
mamba env create -f environment.yml -y -p <some folder ex. ~/.conda/envs/msds2025-ml3-review`>
```

## Error Resolutions

Upon installation, you may encounter an error similar to the one below during model training with tensorflow models:

```
libdevice not found at ./libdevice.10.bc
```

This error happens due to Keras automatically using this `libdevice` driver related to XLA.

To resolve this, execute the following in the terminal:

```
conda activate msds2025-ml3-review
mkdir -p $CONDA_PREFIX/lib/nvvm/libdevice/
ln -s $CONDA_PREFIX/lib/libdevice.10.bc $CONDA_PREFIX/lib/nvvm/libdevice/
```

This creates a soft-link of the `libdevice.10.bc` on a directory which we will specify via environment variables.

We do this by specifying the `XLA_FLAGS` variable via the `os.environ` in our notebook that uses the `msds2025-ml3-review` kernel

```
os.environ['XLA_FLAGS'] = f"--xla_gpu_cuda_data_dir={os.environ['CONDA_PREFIX']}/lib/"
```

Notice that this is included in the header of the RNN notebooks.

Reference: https://skeptric.com/tensorflow-conda/

