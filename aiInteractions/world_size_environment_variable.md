# WORLD_SIZE Environment Variable

Asked Asked Claude Opus 4.5: what does the environment variable WORLD_SIZE represent?

## Overview

The `WORLD_SIZE` environment variable represents **the total number of processes** participating in a distributed training setup using PyTorch DDP (Distributed Data Parallel).

## Distributed Training Context

In multi-GPU or multi-node training, there are three key environment variables:

- **`RANK`**: The global rank of the process across all nodes (0, 1, 2, ..., total_processes-1)
- **`LOCAL_RANK`**: The rank within a single node (which GPU on this machine)
- **`WORLD_SIZE`**: The total number of processes across all nodes

## Example

| Setup | WORLD_SIZE |
|-------|------------|
| Single GPU | 1 |
| 4 GPUs on 1 machine | 4 |
| 8 GPUs on 2 machines (4 each) | 8 |
| 16 GPUs on 4 machines (4 each) | 16 |

## Usage in Training

`WORLD_SIZE` is commonly used to:

1. **Scale the effective batch size**: Total tokens per step = tokens_per_device × WORLD_SIZE
2. **Distribute data**: Each rank processes a subset of the data
3. **Synchronize gradients**: All-reduce operations across all processes
4. **Calculate metrics**: Average losses across all ranks

## Implementation in Nanochat

In [nanochat/common.py](../nanochat/common.py#L137):

```python
def get_dist_info():
    if is_ddp_requested():
        assert all(var in os.environ for var in ['RANK', 'LOCAL_RANK', 'WORLD_SIZE'])
        ddp_rank = int(os.environ['RANK'])
        ddp_local_rank = int(os.environ['LOCAL_RANK'])
        ddp_world_size = int(os.environ['WORLD_SIZE'])
        return True, ddp_rank, ddp_local_rank, ddp_world_size
    else:
        return False, 0, 0, 1  # Default world_size is 1 for single process
```

In [scripts/base_train.py](../scripts/base_train.py#L115-L116), it's used to calculate total tokens per training step:

```python
tokens_per_fwdbwd = args.device_batch_size * args.max_seq_len  # tokens per iteration for a single rank
world_tokens_per_fwdbwd = tokens_per_fwdbwd * ddp_world_size   # total tokens per iteration for all ranks
```

## Key Points

- When not using distributed training, `WORLD_SIZE` defaults to 1
- The effective batch size scales linearly with `WORLD_SIZE`
- Gradient synchronization (all-reduce) happens across all `WORLD_SIZE` processes
