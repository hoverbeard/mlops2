# MLOps Project 2

This repository is part of Project 2 of the HSLU MLOPS module. It contains Dockerized configuration to finetune distilbert-base-uncased on MRPC, with both CPU-only and GPU-enabled Docker images available.  

## Prerequisites

Make sure Docker is installed on your system. You can download and follow the installation instructions for Docker [here](https://docs.docker.com/get-docker/).

## Quick Start Guide

### Option 1: Run the Pre-Built Docker Images

If you want to run the pre-built images directly without building them yourself, use the following commands:

#### For CPU-only:
```bash
docker pull philhova/mlops2:1.1-cpu
docker run philhova/mlops2:1.1-cpu --wandb_key <your_wandb_key>
```

#### For GPU (assuming CUDA-enabled hardware):
```bash
docker pull philhova/mlops2:1.1-gpu
docker run --gpus all philhova/mlops2:1.1-gpu --wandb_key <your_wandb_key>
```

### Option 2: Build the Docker Images Locally First

If you prefer to build the images locally, first clone this repository:

```bash
git clone https://github.com/hoverbeard/mlops2.git
cd mlops2
```

You can then build the images yourself by using the provided Dockerfiles:

#### Build the CPU-only Image:
```bash
docker build -f Dockerfile_cpu -t yournamespace/mlops2:1.1-cpu .
```

#### Build the GPU Image:
```bash
docker build -f Dockerfile_gpu -t yournamespace/mlops2:1.1-gpu .
```
> **Note**: Replace `yournamespace` with your Docker Hub username if you plan to push the images to your Docker Hub account.

The built images can then be run with `docker run` as in Option 1.

## Usage and Arguments

The `main.py` training script accepts several arguments to customize the training configuration, including hyperparameters. Note that the hyperparameter values default to the best configuration recorded in Project 1 and require around 10-15 GB of virtual memory with `batch_size 256`. 

Here is a list of all the available arguments:

| Argument               | Type   | Default                     | Description                                    |
|------------------------|--------|-----------------------------|------------------------------------------------|
| `--wandb_key`          | str    | **Required**                | WandB API key for experiment tracking          |
| `--project_name`       | str    | `"MLOPS_Project2"`          | WandB project name                             |
| `--saved_model_dirpath`| str    | `"./saved_models/"`         | Directory path to save trained models          |
| `--batch_size`         | int    | `256`                       | Batch size for training                        |
| `--accumulate_grad_batches` | int | `1`                      | Number of gradient accumulation steps          |
| `--weight_decay`       | float  | `0.0`                       | Weight decay for the optimizer                 |
| `--warmup_steps`       | int    | `3`                         | Number of warmup steps                         |
| `--lr`                 | float  | `0.0002763964247502885`     | Learning rate                                  |
| `--eps`                | float  | `1e-9`                      | Epsilon value for optimizer                    |
| `--beta1`              | float  | `0.9`                       | Beta1 value for optimizer                      |
| `--beta2`              | float  | `0.999`                     | Beta2 value for optimizer                      |
| `--epochs`             | int    | `3`                         | Number of training epochs                      |

### Example Command
Below is an example command showing how to run the Docker container with custom arguments:

```bash
docker run philhova/mlops2:1.1-cpu \
    --wandb_key <your_wandb_key> \
    --batch_size 128 \
    --lr 0.0001382 \
    --epochs 5 \
    --saved_model_dirpath "/path/to/save/models"
```
