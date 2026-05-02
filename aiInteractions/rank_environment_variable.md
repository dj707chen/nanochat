# RANK Environment Variable

Asked Claude Sonnet 4.5: what does the environment variable RANK represent?

## Overview

The `RANK` environment variable represents **the global rank** (unique identifier) of the current process in a distributed training setup using PyTorch DDP (Distributed Data Parallel).

## Distributed Training Context

In multi-GPU or multi-node training, there are three key environment variables:

- **`RANK`**: The global rank of the process across all nodes (0, 1, 2, ..., total_processes-1)
- **`LOCAL_RANK`**: The rank within a single node (which GPU on this machine)
- **`WORLD_SIZE`**: The total number of processes across all nodes

## Example

If you have 2 machines with 4 GPUs each:

| Machine | GPU | RANK | LOCAL_RANK |
|---------|-----|------|------------|
| Machine 1 | GPU 0 | 0 | 0 |
| Machine 1 | GPU 1 | 1 | 1 |
| Machine 1 | GPU 2 | 2 | 2 |
| Machine 1 | GPU 3 | 3 | 3 |
| Machine 2 | GPU 0 | 4 | 0 |
| Machine 2 | GPU 1 | 5 | 1 |
| Machine 2 | GPU 2 | 6 | 2 |
| Machine 2 | GPU 3 | 7 | 3 |

## Master Process

The rank=0 process is typically the "master process" that handles:
- Logging
- Checkpointing
- Other single-process tasks

All other ranks participate in training but don't perform these extra duties.

## Implementation in Nanochat

In the codebase, `RANK` is used in [nanochat/common.py](../nanochat/common.py#L135):

```python
def get_dist_info():
    if is_ddp_requested():
        assert all(var in os.environ for var in ['RANK', 'LOCAL_RANK', 'WORLD_SIZE'])
        ddp_rank = int(os.environ['RANK'])
        ddp_local_rank = int(os.environ['LOCAL_RANK'])
        ddp_world_size = int(os.environ['WORLD_SIZE'])
        return True, ddp_rank, ddp_local_rank, ddp_world_size
    else:
        return False, 0, 0, 1
```

The function checks if DDP is requested and reads these environment variables to configure distributed training properly.
