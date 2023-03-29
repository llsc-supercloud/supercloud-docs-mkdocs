Optimizing your GPU Jobs {#gpu_jobs}
========================

The GPUs on Supercloud often are in very high demand. Before asking for
more GPUs you\'ll want to do what you can to optimize your code to get
the most out of the resources you do have.

The first thing you need to do is profile your code. Time your code both
with and without the GPU. When you are running with a GPU, monitor the
GPU Utilization and GPU Memory use. You can do this by going to the
compute node where it is running (get the node name from LLstat, then
ssh to the node with \"`ssh nodename`\") and run the `nvidia-smi -l`
command (press `Ctrl+C` to exit `nvidia-smi -l `when you are done).

If your GPU utilization and GPU memory use is low, that means you
aren\'t getting the full advantage out of the GPU. If this is the case,
try out some of the suggestions below.

If you are only getting a small speedup, say 2x or 4x, especially after
trying the suggestions below, it may not be worthwhile using GPUs at
all. In general, the CPU nodes on Supercloud are more available so we
will likely be willing to increase your allocation of CPU nodes to make
up for the small loss of speedup switching to CPUs.

Getting More out of the GPUs
----------------------------

If your GPU utilization and memory use are low, usually this means you
might be able to get some more performance out of the GPUs. There are a
few things you can try if you are training machine learning models:

-   Increase the batch size.
-   Enable CUDA kernel tuning.
    -   For Pytorch, add this before the training starts:
        `torch.backends.cudnn.benchmark = True`
    -   For Tensorflow, set this environment variable in your submission
        script: `export TF_CUDNN_USE_AUTOTUNE=1`
-   If you are using Tensorflow you can also try mixed-precision
    training (we haven't played with this in Pytorch, but it could be
    possible).
    -   Tensorflow 2.4.1 and newer (anaconda/2021a)
        -   Add the following to the beginning of your Python code:
            -   `from tensorflow.keras import mixed_precision`
            -   `policy = mixed_precision.Policy('mixed_float16')`
            -   `mixed_precision.set_global_policy(policy)`
    -   Pre-Tensorflow 2.4.1 (anaconda/2020b and older)
        -   To do so set the following environment variables in your
            submission script:
            -   `export TF_ENABLE_AUTO_MIXED_PRECISION_GRAPH_REWRITE=1`
            -   `export TF_ENABLE_AUTO_MIXED_PRECISION=1`
        -   And add this to your Python code, here `opt` is the
            optimizer object:
            -   `opt = tf.train.experimental.enable_mixed_precision_graph_rewrite(opt)`
-   If the number of filters in the model layers is a multiple of 64,
    you will see an additional speedup because the V100 tensor cores are
    optimized for matmul/conv ops of these sizes.
-   If both GPU utilization and memory are below 50% you could try to
    train two models per GPU. This isn't too hard to do with `Triples
    Mode <job_array_triples>`{.interpreted-text role="ref"}, simply set
    the number of processes per node to 4.

Requesting More GPUs
--------------------

If you have tried the above suggestions and you still find you need more
GPUs you can put in a request. Send an email to
[supercloud\@mit.edu](mailto:supercolud@mit.edu) and tell us what you
need, what you need it for, and how long you need the allocation
increase. Are you trying to run a large distributed training run, or are
you doing hyperparameter searches? Justify your request with numbers:
show us that you have compared CPU and GPU run times, that you have good
GPU utilization, and that you have tried the above suggestions. Explain
how you got to the number of GPUs you are requesting. If the system is
not too busy we may grant your request. Keep in mind that others are
likely to have the same paper deadlines as you and we may not be able to
grant requests during these busy periods, so plan ahead.
