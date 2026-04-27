These scripts are a model-library janitor for your various self hosted models

Its default run scans <your model directory>, recognizes LM Studio/Ollama/GPT4all/AI Navigator/Jan/AnythingLLM manifests and loose model folders, fixes misplaced model files into publisher/model layout, checks Hugging Face caches for placeholder downloads, and registers GGUF/blob-backed models the appropriate application.

With extra flags, it can also migrate HF cache models, validate model integrity, flatten long model names into short local names, remove partial downloads, and delete duplicates.

The biggest operational risk is –dedupe. The second biggest is –cleanup-hf-source. The third is normal non-dry-run structure fixing, because it moves files.
