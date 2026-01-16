# Running Jupyter Notebooks Outside VS Code

AI generated for prompt: how can I run demo.ipynb outside of vs code?


There are several ways to run the demo.ipynb notebook outside of VS Code:

## 1. Using Jupyter Notebook (Browser-based)

```bash
# Install jupyter if not already installed
uv add --dev jupyter

# Launch Jupyter Notebook server
uv run jupyter notebook demo.ipynb
```

This will open the notebook in your web browser.

## 2. Using Jupyter Lab (Modern interface)

```bash
# Install jupyterlab
uv add --dev jupyterlab

# Launch Jupyter Lab
uv run jupyter lab demo.ipynb
```

## 3. Convert to Python script and run

```bash
# Convert notebook to Python script
uv run jupyter nbconvert --to script demo.ipynb

# Run the generated Python file
uv run python demo.py
```

## 4. Execute notebook from command line (no GUI)

```bash
# Install nbconvert if needed
uv add --dev nbconvert

# Execute the notebook and save outputs
uv run jupyter nbconvert --execute --to notebook --inplace demo.ipynb

# Or execute and export to HTML
uv run jupyter nbconvert --execute --to html demo.ipynb
```

## 5. Using papermill (for programmatic execution)

```bash
# Install papermill
uv add --dev papermill

# Execute and save to a new notebook
uv run papermill demo.ipynb demo_output.ipynb
```

## Recommended Approach

The most common and user-friendly approach is **option 1 or 2** (Jupyter Notebook/Lab in a browser).
