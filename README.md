# Optimizing-GPU-Utilization-at-Training

This repository contains a collection of experiments demonstrating various optimization techniques for PyTorch models on GPU. Each optimization technique is implemented in a separate Python file with a self-contained training loop, making it easy to compare the performance impact of different approaches.

## Overview

Training deep learning models efficiently requires understanding how to optimize GPU utilization. This project provides a practical exploration of various optimization techniques, with each implementation focusing on a specific optimization strategy. Performance metrics are collected and can be visualized to compare the effectiveness of different approaches.

## Project Structure

- `no_optimization.py`: Baseline implementation with no optimizations
- `tensorFloat32.py`: Implementation using TensorFloat-32 (TF32) precision
- `brainFloat16.py`: Implementation using BFloat16 precision
- `torch_compile.py`: Implementation using torch.compile
- `flash_attention.py`: Implementation using FlashAttention
- `fused_optimizer.py`: Implementation using fused Adam optimizer
- `Utils/`: Directory containing utility functions for setting up data, model, etc.
- `Makefile`: Automation script for running experiments and generating comparisons

## Installation

### Prerequisites

- Python 3.8+
- CUDA-enabled GPU (for optimal results)

### Dependencies

```bash
pip install torch torchvision matplotlib pandas
```

## Usage

This project includes a Makefile that simplifies running experiments and generating comparisons.

### Running Individual Optimization Techniques

To run a specific optimization technique:

```bash
make baseline    # Run baseline (no optimization)
make tf32        # Run TensorFloat32 optimization
make bf16        # Run BrainFloat16 optimization 
make torch_compile    # Run torch.compile optimization
make flash       # Run FlashAttention optimization
make fused       # Run fused optimizer optimization
```

### Running All Optimization Techniques

To run all optimization techniques sequentially:

```bash
make all
```

### Generating Comparison Plots

After running one or more experiments:

```bash
make plot
```

This will create three types of plots:
- Line plot showing performance over steps for different optimizations
- Heatmap visualization of the performance data
- Horizontal bar chart showing mean relative speedup compared to no optimization

### Benchmarking

To run a benchmarking suite with all optimizations:

```bash
make benchmark
```

This will run the optimizations with the default number of steps and double the steps, generating plots with prefixes to distinguish between the runs.

### Additional Commands

```bash
make compare_pairs    # Run all techniques and compare them
make reset            # Reset results file and plots
make clean            # Remove generated files
make results          # Display summary of current results
```

### Command-line Options

The Makefile supports the following options:

```
STEPS=n               # Set number of steps (default: 50)
```

Examples:

```bash
make baseline STEPS=30
make all STEPS=100
make plot
```

## Results

Results are saved to `results.csv` with timing information for each optimization technique. When generating plots, comparison charts will be created showing the relative performance of each technique.

## Optimizations Implemented

1. **No Optimization**: Baseline implementation with default settings
2. **TensorFloat-32**: Using TF32 precision on Ampere (and newer) GPUs
3. **BrainFloat16**: Using BFloat16 precision for reduced memory usage and faster training
4. **Torch Compile**: Using torch.compile for just-in-time compilation
5. **Flash Attention**: Implementing FlashAttention for faster and more memory-efficient attention
6. **Fused Optimizer**: Using a fused optimizer implementation for reduced kernel launches

## Performance Considerations

- **TensorFloat-32 (TF32)** provides a good balance between precision and speed for most deep learning workloads.
- **BFloat16** offers reduced memory usage and can significantly speed up training on supported hardware.
- **torch.compile** can improve performance through just-in-time compilation but may have compilation overhead.
- **FlashAttention** is particularly effective for transformer models with long sequence lengths.
- **Fused Optimizers** reduce GPU kernel launches and memory operations, improving overall training efficiency.

## References

- [NVIDIA Ampere Architecture Whitepaper](https://images.nvidia.com/aem-dam/en-zz/Solutions/data-center/nvidia-ampere-architecture-whitepaper.pdf)
- [PyTorch Documentation on torch.compile](https://pytorch.org/tutorials/intermediate/torch_compile_tutorial.html)
- [PyTorch Documentation on Automatic Mixed Precision](https://pytorch.org/docs/stable/amp.html)
- [FlashAttention Paper](https://arxiv.org/abs/2205.14135)

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. 