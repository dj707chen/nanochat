# Tokenizer Usage Analysis

## Question
Will the tokenizer trained at [speedrun.sh#L62](../speedrun.sh#L62) be used by [chat_web.py](../scripts/chat_web.py)?

## Answer
**Yes**, the same tokenizer will be used.

## Explanation

### 1. Tokenizer Training
- **Location**: [speedrun.sh#L62](../speedrun.sh#L62)
- **Command**: `python -m scripts.tok_train --max-chars=2000000000 --vocab-size=65536`
- **Saves to**: `{base_dir}/tokenizer/` directory
  - See [tok_train.py#L57](../scripts/tok_train.py#L57): `tokenizer.save(tokenizer_dir)`
  - Where `tokenizer_dir = os.path.join(base_dir, "tokenizer")`

### 2. Tokenizer Loading in chat_web.py
The loading chain is as follows:

1. **[chat_web.py#L126](../scripts/chat_web.py#L126)**:
   ```python
   model, tokenizer, _ = load_model(source, device, phase="eval", model_tag=model_tag, step=step)
   ```

2. **[checkpoint_manager.py#L112](../nanochat/checkpoint_manager.py#L112)**:
   ```python
   tokenizer = get_tokenizer()
   ```

3. **[tokenizer.py#L390-L394](../nanochat/tokenizer.py#L390-L394)**:
   ```python
   def get_tokenizer():
       from nanochat.common import get_base_dir
       base_dir = get_base_dir()
       tokenizer_dir = os.path.join(base_dir, "tokenizer")
       return RustBPETokenizer.from_directory(tokenizer_dir)
   ```

### 3. Conclusion
Both the training script ([tok_train.py](../scripts/tok_train.py)) and the web chat server ([chat_web.py](../scripts/chat_web.py)) use the **same tokenizer directory**: `{base_dir}/tokenizer/`.

Therefore, the tokenizer with **vocab_size=65536** trained in [speedrun.sh](../speedrun.sh) is exactly the tokenizer that [chat_web.py](../scripts/chat_web.py) loads and uses for inference.
