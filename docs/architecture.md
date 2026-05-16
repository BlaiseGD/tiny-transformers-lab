\*Disclaimer: this document was produced by ChatGPT for initial setup 



🧠 Tiny Transformer Lab — Project Structure Overview



This project is organized as a modular deep learning + CUDA systems implementation of a transformer model. The goal is to build a full training and inference pipeline from first principles, while progressively accelerating key components with custom CUDA kernels.



The structure is split into five main layers:



1\. src/ — Core Implementation



This is the heart of the project. It contains all executable logic.



src/main.cpp

&#x09;Entry point of the project.



Responsible for:



initializing model

loading dataset

starting training loop

running inference / sampling



Think of it as:



“the driver script of the entire system”



src/cpu/ — Reference Implementations

&#x09;This folder contains correct, readable, CPU-based versions of all neural network components.



Purpose:



serve as ground truth for correctness

allow easy debugging before CUDA acceleration

provide baseline performance



Modules:



tensor.cpp

&#x09;core tensor abstraction (shape, storage, indexing)

linear.cpp

&#x09;fully connected layers

attention.cpp

&#x09;reference attention implementation

transformer.cpp

&#x09;full forward pass composition

layernorm.cpp

&#x09;normalization layer

softmax.cpp

&#x09;stable softmax implementation

embedding.cpp

&#x09;token/positional embeddings



👉 These are your “slow but correct” versions.



src/cuda/ — GPU-Accelerated Kernels



Each file replaces or accelerates a CPU equivalent.



Modules:



matmul.cu

&#x09;tiled matrix multiplication (core of everything)

reductions.cu

&#x09;sum/reduction primitives (used in softmax, norm, etc.)

softmax.cu

&#x09;numerically stable GPU softmax

layernorm.cu

&#x09;fused normalization kernel

attention.cu

&#x09;scaled dot-product attention kernel

embedding.cu

&#x09;embedding lookup / positional encoding acceleration



👉 This folder is where you apply:



shared memory optimization

warp-level primitives

occupancy tuning

kernel fusion

Nsight profiling iteration

src/training/ — Optimization + Learning Loop



This handles the ML training system.



Modules:



dataloader.cpp

&#x09;batching, shuffling, sequence preparation

optimizer.cpp

&#x09;SGD / Adam implementation

loss.cpp

&#x09;cross-entropy / token loss

train.cpp

&#x09;training loop orchestration



👉 This is where:



forward pass meets backward pass

gradients are applied

learning actually happens

src/utils/ — Infrastructure Code



Support utilities used across the project:



timer.cpp

&#x09;benchmarking CPU/GPU execution time

logger.cpp

&#x09;structured logging (loss, throughput, etc.)



👉 This keeps the core logic clean and measurable.



2\. include/ — Public Interfaces



Header files mirroring src/.



Purpose:



define clean interfaces

separate implementation from declarations

allow modular compilation

make CUDA/C++ interop cleaner



Structure mirrors src/:



cpu/

cuda/

training/

utils/



👉 Think of this as the “API surface” of your framework.



3\. docs/ — Writeups



Suggested content:



architecture.md

&#x09;overall system design

attention.md

&#x09;explanation + math + implementation notes

cuda-kernels.md

&#x09;optimization strategies (tiling, warps, memory coalescing)

profiling.md

&#x09;Nsight results, bottlenecks, fixes

benchmarks.md

&#x09;CPU vs GPU comparisons



4\. datasets/ — Training Data



Stores input data for training.



Examples:



Tiny Shakespeare

WikiText subsets

custom text corpora

tokenized datasets (later stage)



👉 Keep raw + processed data separated if it grows.



5\. benchmarks/ — Performance Tracking



Used for:



CPU vs CUDA comparisons

kernel speedups

memory throughput analysis

regression tracking



This is where you show:



“we made it N× faster after optimization”



6\. tests/ — Correctness Validation



Critical for CUDA projects.



You’ll use this for:



tensor correctness checks

gradient checking (later)

CPU vs GPU output validation

numerical stability tests



👉 This prevents “fast but wrong” kernels.



7\. scripts/ — Automation Layer



Helper scripts for:



training runs

dataset preprocessing

benchmarking runs

plotting results



Likely Python later.

