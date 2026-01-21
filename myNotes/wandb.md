# wandb
https://github.com/dj707chen/wandb

username: weiping-chen
    sign in via Google weiping.chen@gmail.com
Organization URL: wandb.ai/weiping-chen-personal

https://wandb.ai/weiping-chen-personal

## API Key

    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

created on 01/18/26

log a run
```python
import random
import wandb
# Start a new wandb run to track this script.
run = wandb.init(
    # Set the wandb entity where your project will be logged (generally your team name).
    entity="weiping-chen-personal",
    # Set the wandb project where this run will be logged.
    project="my-awesome-project",
    # Track hyperparameters and run metadata.
    config={
        "learning_rate": 0.02,
        "architecture": "CNN",
        "dataset": "CIFAR-100",
        "epochs": 10,
    },
)
# Simulate training.
epochs = 10
offset = random.random() / 5
for epoch in range(2, epochs):
    acc = 1 - 2**-epoch - random.random() / epoch - offset
    loss = 2**-epoch + random.random() / epoch + offset

    # Log metrics to wandb.
    run.log({"acc": acc, "loss": loss})
# Finish the run and upload any remaining data.
run.finish()
```

## Installation

```shell
pip install wandb
wandb login
```
