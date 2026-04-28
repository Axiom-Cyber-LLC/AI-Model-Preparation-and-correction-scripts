# 🧹 ***Local Model Library Janitor Scripts***

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Platform](https://img.shields.io/badge/platform-macOS-blue)
![Python](https://img.shields.io/badge/python-3.11%2B-yellow)
![Safety](https://img.shields.io/badge/safety-run%20dry--run%20first-orange)
![Models](https://img.shields.io/badge/models-GGUF%20%7C%20Safetensors%20%7C%20MLX-purple)

A toolkit for cleaning, organizing, validating, registering, and sharing local model files across self-hosted and desktop AI applications.

These scripts are a **model-library janitor** for your local model collection.

They help prepare models for:

- **LM Studio**
- **Ollama**
- **GPT4All**
- **Anaconda AI Navigator**
- **Jan**
- **AnythingLLM**
- **Local OpenAI-compatible coding agents**

They are designed for users who keep models on a large external drive, shared model folder, or local model cache and want multiple local AI apps to use the same model files without duplicating hundreds of gigabytes of weights.

## 🟢 ***What These Scripts Do***

A normal run can:

- Scan your local model directory
- Read LM Studio and Ollama-style manifests
- Detect loose model files
- Detect publisher/model folder layouts
- Detect Hugging Face cache-style model directories
- Detect GGUF, Safetensors, PyTorch, and MLX-style model layouts
- Fix misplaced model files into a cleaner `publisher/model` layout
- Check Hugging Face caches for placeholder Git LFS downloads
- Report models that need to be fully downloaded
- Register GGUF or blob-backed models with Ollama
- Create symlinks for GPT4All and Jan
- Point AnythingLLM Desktop at an external/system Ollama instance
- Register flat-dir models in Anaconda AI Navigator 
- Build a short-name flattening plan for long model names
- Apply a flattening plan using hardlinks or copies
- Validate model integrity
- Remove partial download residue
- Optionally delete duplicates

The scripts are meant to make one local model library usable across multiple frontends.

The core idea is:

1. Keep the actual large model files in one place.
2. Let each app see the models in the layout it expects.
3. Avoid copying the same model weights repeatedly.
4. Use symlinks, hardlinks, manifests, or app-specific registration where possible.

## 🧩 ***Supported Applications***

| Application | What the scripts do |
|---|---|
| **LM Studio** | Organizes loose model folders and reads LM Studio/Ollama-style manifests |
| **Ollama** | Registers GGUF models using `ollama create` |
| **GPT4All** | Creates symlinks to local GGUF files inside GPT4All’s model folder |
| **Jan** | Creates symlinks to local GGUF and MLX models inside Jan’s model directories |
| **AnythingLLM** | Points AnythingLLM Desktop at external/system Ollama |
| **AI Navigator** | Registers flat-dir local models in the AI Navigator database |
| **Local coding agents** | Provides a wrapper that lets local OpenAI-compatible models write code to disk |

## 🧠 ***Model Types Recognized***

The scripts recognize common local model file types, including:

| Type | Extensions / layout |
|---|---|
| **GGUF** | `.gguf` |
| **Safetensors** | `.safetensors` |
| **PyTorch legacy** | `.bin`, `.pt`, `.pth` |
| **MLX-style models** | MLX model directories with metadata and weights |
| **Hugging Face cache models** | `models--owner--repo/snapshots/<commit>/...` |
| **Ollama blob-backed models** | Ollama-style manifests pointing to SHA256 blobs |

Common companion files are preserved when moving or exporting models, including:

- `config.json`
- `tokenizer.json`
- `tokenizer_config.json`
- `tokenizer.model`
- `special_tokens_map.json`
- `generation_config.json`
- `preprocessor_config.json`
- `added_tokens.json`
- `vocab.json`
- `vocab.txt`
- `merges.txt`
- `model.safetensors.index.json`
- `pytorch_model.bin.index.json`
- `chat_template.json`
- `quantize_config.json`
- `weights.json`

  ## 🧭 ***Configure the Scripts for Your Environment***

Some default paths in these scripts may point to the original author’s local machine.

For example:

/Volumes/SamsungSSDE/models

/Volumes/SamsungSSDE/models-flat

/Volumes/SamsungSSDE/.cache/huggingface

/Volumes/SamsungSSDE/.cache/modelscope

These are **example/default paths**, not universal paths.

Before running the scripts live, identify where your own models are stored and either:

1. Pass your paths with command-line options, or
2. Edit the default path constants inside the scripts.

The safest method is to use command-line options whenever available.

## 🟡 ***About Example Paths***

Any path that starts with:

/Volumes/...

is a **macOS external-drive example path**.

Do not copy it literally unless that path exists on your machine.

When you see:
/Volumes/YourDriveName/models-flat

read it as:
"The file path to your flat model directory."

When you see:
/Volumes/YourDriveName/models

read it as:
"The file path to your main model directory."

When you see:
/Volumes/YourDriveName/.cache/huggingface

read it as:
"The file path to your Hugging Face cache directory."

When you see:
/Volumes/YourDriveName/.cache/modelscope

read it as:
"The file path to your ModelScope cache directory."

Replace these example paths with the real paths from your own environment.

## 🟡 ***Path Placeholders***

This README uses generic placeholders instead of author-specific paths.

| Placeholder | Meaning |
|---|---|
| `<FLAT_MODEL_DIR>` | The file path to your flat model directory |
| `<MODEL_DIR>` | The file path to your main model directory |
| `<GGUF_MODEL_DIR>` | The file path to a directory containing GGUF models |
| `<MLX_MODEL_DIR>` | The file path to a directory containing MLX models |
| `<HF_CACHE_DIR>` | The file path to your Hugging Face cache directory |
| `<MODELSCOPE_CACHE_DIR>` | The file path to your ModelScope cache directory |
| `<AI_NAVIGATOR_MODEL_DIR>` | The file path to AI Navigator’s model directory |
| `<GPT4ALL_MODEL_DIR>` | The file path to GPT4All’s model directory |
| `<JAN_GGUF_MODEL_DIR>` | The file path to Jan’s llama.cpp/GGUF model directory |
| `<JAN_MLX_MODEL_DIR>` | The file path to Jan’s MLX model directory |
| `<SANDBOX_DIR>` | A disposable project folder where an agent can safely write files |

Example only:

/Volumes/YourDriveName/models-flat

means:

<FLAT_MODEL_DIR>

So this command:

python3 Prepare_models_for_Lmstudio.py --input "/Volumes/YourDriveName/models-flat"

should be understood as:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>"

## 🔍 ***How to Find Your Model Directory***

If you are not sure where your models are, check common locations.

### macOS

ls -la "$HOME/.lmstudio/models"

ls -la "$HOME/.cache/huggingface/hub"

ls -la "/Volumes"

### Ollama

ollama list

Ollama’s internal model storage is normally managed by Ollama itself. These scripts mainly register external GGUF files into Ollama.

### Hugging Face cache

echo "$HF_HOME"

echo "$HF_HUB_CACHE"

ls -la "$HOME/.cache/huggingface/hub"

### LM Studio

Open LM Studio and check:

Settings → General → Models Directory

Use that path with:

python3 Prepare_models_for_Lmstudio.py --input "<LM_STUDIO_MODEL_DIR>" --dry-run

## 🧭 ***Per-Script Path Configuration***

| Script | Path option users should set for their environment |
|---|---|
| `Prepare_models_for_Lmstudio.py` | `--input "<FLAT_MODEL_DIR>"` or `--input "<MODEL_DIR>"` |
| `Prepare_models_for_Ollama.py` | `--root "<GGUF_MODEL_DIR>"` |
| `Prepare_models_for_AIStudio.py` | `--plan "<FLAT_MODEL_DIR>/rename-plan.json"` and optionally `--target "<AI_NAVIGATOR_MODEL_DIR>"` |
| `Prepare_models_for_GPT4All.py` | `--gguf-root "<GGUF_MODEL_DIR>"` and optionally `--target "<GPT4ALL_MODEL_DIR>"` |
| `Prepare_models_for_Jan.py` | `--gguf-root "<GGUF_MODEL_DIR>"` and/or `--mlx-root "<MLX_MODEL_DIR>"` |
| `Prepare_models_for_AnythingLLM.py` | Usually no model path; set `OLLAMA_HOST` if Ollama is not at `http://localhost:11434` |
| `agent_filesystem_wrapper.py` | `--root "<SANDBOX_DIR>"` |

## ✅ ***Example Path Translation***

If your flat model directory is:
/Volumes/MySSD/models-flat

then use it wherever the README says:
<FLAT_MODEL_DIR>

For example:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dry-run

becomes:
python3 Prepare_models_for_Lmstudio.py --input "/Volumes/MySSD/models-flat" --dry-run

If your models are under your home directory, then:

<FLAT_MODEL_DIR>

might become:
$HOME/models-flat

Example:

python3 Prepare_models_for_Lmstudio.py --input "$HOME/models-flat" --dry-run

## 🔴 ***Important Safety Notes***

Always run with `--dry-run` first.

Recommended first run for any script:

python3 SCRIPT_NAME.py --dry-run

Then run live only after reviewing the output:

python3 SCRIPT_NAME.py

### Highest-Risk Options

| Risk Level | Option | Why it matters |
|---|---|---|
| 🔴 High | `--dedupe` | Can delete duplicate-looking models |
| 🔴 High | `--cleanup-hf-source` | Deletes original Hugging Face cache-style source folders after migration |
| 🟠 Medium | Normal non-dry-run structure fixing | May move misplaced model files |
| 🟠 Medium | `--clean-partial-downloads` | Deletes partial download residue from cache blob folders |
| 🟠 Medium | Agent wrapper `run_shell` | Lets the model run shell commands |

> [!WARNING]
> Do not use `--dedupe` unless you have backups and understand your model layout.

> [!WARNING]
> Do not use `--cleanup-hf-source` unless you are sure the migration succeeded and you no longer need the original Hugging Face cache folders.

> [!WARNING]
> The agent filesystem wrapper can expose shell execution. Direct file tools are root-confined, but shell commands are not a true sandbox.

## 💾 ***What May Change on Disk***

Depending on the script and flags used, the toolkit may:

- Move misplaced model files
- Create missing publisher/model directories
- Create symlinks for GPT4All or Jan
- Create hardlinks or copies when applying a flatten plan
- Create or update Ollama model registrations
- Write an Ollama state cache
- Edit AnythingLLM’s `.env` file
- Back up and modify AI Navigator LokiJS database
- Create an AI Navigator-compatible symlink farm
- Write `rename-plan.json`
- Delete partial download residue
- Delete duplicate models if `--dedupe` is used
- Delete Hugging Face source folders if `--cleanup-hf-source` is used

Original model weights are usually reused through symlinks, hardlinks, or app-specific registration where possible.

## ✅ ***Recommended Workflow***

Use this workflow for every script:

1. Run a dry-run first.
2. Read the output carefully.
3. Confirm the paths are correct.
4. Confirm the script is touching the expected files.
5. Run live only when the dry-run output looks safe.

Example:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dry-run

Then:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>"

For destructive options, use extra caution:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dedupe --dry-run

Only run live if the output is exactly what you expect:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dedupe

# 📦 ***Scripts Included***

| Script | Purpose |
|---|---|
| `Prepare_models_for_Lmstudio.py` | Prepare LM Studio-style model folders and optionally register GGUF/blob-backed models with Ollama |
| `Prepare_models_for_Ollama.py` | Register GGUF models with Ollama |
| `Prepare_models_for_AIStudio.py` | Register flat-dir models in Anaconda AI Navigator |
| `Prepare_models_for_AnythingLLM.py` | Point AnythingLLM Desktop at external/system Ollama |
| `Prepare_models_for_GPT4All.py` | Symlink local GGUF files into GPT4All |
| `Prepare_models_for_Jan.py` | Symlink local GGUF and MLX models into Jan |
| `agent_filesystem_wrapper.py` | Let local OpenAI-compatible models write code to disk through wrapper tools |

# 🟦 ***1. Prepare_models_for_Lmstudio.py***

Prepares models under an LM Studio-style model directory and can optionally register GGUF or blob-backed models with Ollama.

Default model directory should be treated as:

<FLAT_MODEL_DIR>

## ***What It Does***

This script can:

- Scan LM Studio/Ollama-style manifests
- Resolve blob-backed models from manifests
- Scan loose model folders
- Detect standalone `.gguf` files
- Detect Safetensors and PyTorch-style model directories
- Move misplaced model files into `publisher/model` layout
- Detect Git LFS pointer files
- Report missing blobs
- Report broken models
- Scan Hugging Face cache roots for models that still need downloading
- Optionally migrate Hugging Face cache-style folders
- Optionally validate model integrity
- Optionally build a short-name flatten plan
- Optionally apply a flatten plan using hardlinks or copies
- Optionally clean partial downloads
- Optionally delete duplicates
- Optionally register GGUF/blob-backed models with Ollama

## ***Normal Use***

Preview only:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dry-run

Live run:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>"

## ***Options for Prepare_models_for_Lmstudio.py***

| Option | What the user enters | What it does |
|---|---|---|
| `--input PATH` | `<FLAT_MODEL_DIR>` or `<MODEL_DIR>` | Uses a specific model directory |
| `--dry-run` | Nothing | Previews actions without applying most changes |
| `--no-ollama` | Nothing | Skips Ollama registration |
| `--force` | Nothing | Re-registers all Ollama models even if previously cached |
| `--migrate-hf-cache` | Nothing | Converts `models--owner--repo` cache folders into `owner/repo` folders |
| `--cleanup-hf-source` | Nothing | Deletes original `models--owner--repo` folders after successful migration |
| `--clean-partial-downloads` | Nothing | Removes abandoned partial download files from cache blob folders |
| `--also-scan PATH` | Extra cache folder | Adds another directory to scan for partial downloads |
| `--validate` | Nothing | Checks model integrity |
| `--flatten` | Nothing | Builds a short-name flattening plan |
| `--flatten-output PATH` | Destination folder | Writes flattened layout and `rename-plan.json` there |
| `--flatten-input PATH` | Extra model source folder | Adds another source root to flatten planning |
| `--apply-flatten PLAN.json` | Path to `rename-plan.json` | Applies a previously generated flatten plan |
| `--workers NUMBER` | Integer, usually 8 or 16 | Parallel workers for flatten application |
| `--ollama-workers NUMBER` | Integer, usually 4 | Parallel workers for Ollama registration |
| `--dedupe` | Nothing | Deletes duplicate models |

> [!WARNING]
> `--dedupe` is destructive. Avoid it on flat-dir, hardlink, or symlink-farm layouts unless you have backups.

> [!WARNING]
> `--cleanup-hf-source` is destructive. It deletes the original Hugging Face cache-style source folders after migration.

## ***Examples for Prepare_models_for_Lmstudio.py***

Use your flat model directory:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dry-run

Use your main model directory:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --dry-run

Skip Ollama registration:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --no-ollama

Re-register all Ollama models:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --force

Migrate Hugging Face cache folders:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --migrate-hf-cache

Migrate Hugging Face cache folders and delete the original source folders:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --migrate-hf-cache --cleanup-hf-source

Clean partial downloads:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --clean-partial-downloads

Clean partial downloads and scan another cache folder:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --clean-partial-downloads --also-scan "<HF_CACHE_DIR>"

Validate models:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --validate

Build a flatten plan:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --flatten

Build a flatten plan with an extra input root:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --flatten --flatten-input "<GGUF_MODEL_DIR>"

Write the flatten output to a specific folder:
python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --flatten --flatten-output "<FLAT_MODEL_DIR>"

Apply a flatten plan:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --apply-flatten "<FLAT_MODEL_DIR>/rename-plan.json"

Apply a flatten plan with more workers:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --apply-flatten "<FLAT_MODEL_DIR>/rename-plan.json" --workers 16

Run Ollama registration with 4 workers:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --ollama-workers 4

Delete duplicate models:
python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dedupe

## ***Model Validation Checks***

When run with:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --validate

the script checks for:

- Broken symlinks
- Git LFS pointer stubs
- Tiny invalid weight files
- Bad GGUF headers
- Missing Safetensors shards
- Corrupt `model.safetensors.index.json`

For `.gguf` files, it checks that the file begins with the `GGUF` magic bytes.

For sharded Safetensors models, it checks that every shard referenced in `model.safetensors.index.json` exists and has a plausible size.

## ***Flatten Mode***

Flatten mode builds short local names for long model identifiers.

It can convert long Hugging Face-style names into shorter local names and create a plan like:

<FLAT_MODEL_DIR>/rename-plan.json

Build the plan:

python3 Prepare_models_for_Lmstudio.py --input "<MODEL_DIR>" --flatten --flatten-output "<FLAT_MODEL_DIR>"

Apply the plan:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --apply-flatten "<FLAT_MODEL_DIR>/rename-plan.json"

Flatten mode tries to avoid duplicate copies by using hardlinks when source and destination are on the same filesystem. If hardlinks are not possible, it falls back to copying.

Collision handling attempts to disambiguate models by:

- Format
- Author
- Format + author
- Short hash fallback

# 🟦 ***2. Prepare_models_for_Ollama.py***

Registers GGUF models with Ollama.

Default scan roots should be treated as examples. Users should pass their own GGUF model directories.

## ***What It Does***

This script can:

- Scan one or more model roots
- Find local `.gguf` files
- Register GGUF files with Ollama using `ollama create`
- Reuse already registered models when unchanged
- Force re-registration when requested
- Clean orphaned Ollama entries if enabled
- Run in dry-run mode before applying changes

## ***Normal Use***

Preview:

python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>" --dry-run

Live run:

python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>"

## ***Options for Prepare_models_for_Ollama.py***

| Option | What the user enters | What it does |
|---|---|---|
| `--root PATH` | `<GGUF_MODEL_DIR>` | Scans a specific model root instead of defaults |
| `--flat PATH` | Flat model directory | Legacy alias for one flat directory |
| `--force` | Nothing | Re-registers models even if unchanged |
| `--workers NUMBER` | Integer, usually 4 | Number of parallel `ollama create` workers |
| `--clean-orphans` | Nothing | Removes Ollama entries matching orphan cleanup rules |
| `--dry-run` | Nothing | Previews actions |

## ***Examples for Prepare_models_for_Ollama.py***

Scan one model root:
python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>"

Scan multiple roots:
python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>" --root "<MODEL_DIR>"

Use a flat model directory:
python3 Prepare_models_for_Ollama.py --flat "<FLAT_MODEL_DIR>/local"

Force re-registration:
python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>" --force

Use 4 workers:
python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>" --workers 4

Clean orphaned Ollama entries:
python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>" --clean-orphans

# 🟦 ***3. Prepare_models_for_AIStudio.py***

Registers local flat-dir models in Anaconda AI Navigator by editing its LokiJS database.

Default flatten plan should be treated as:

<FLAT_MODEL_DIR>/rename-plan.json

Default flat model files should be treated as:

<FLAT_MODEL_DIR>/local

Default AI Navigator database is usually:

~/Library/Application Support/ai-navigator/ailauncher.db

> [!IMPORTANT]
> Quit AI Navigator before running this script.

## ***What It Does***

This script can:

- Read a flatten `rename-plan.json`
- Register local flat-dir models in AI Navigator
- Modify the AI Navigator LokiJS database
- Create backups before database changes
- Create a Hugging Face-style symlink farm if needed
- Hash model files unless `--no-hash` is used
- Restore the latest database backup with `--revert`

## ***Normal Use***

Preview:

python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json" --dry-run

Live run:

python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json"

## ***Options for Prepare_models_for_AIStudio.py***

| Option | What the user enters | What it does |
|---|---|---|
| `--plan PATH` | `<FLAT_MODEL_DIR>/rename-plan.json` | Uses a specific flatten plan |
| `--target PATH` | `<AI_NAVIGATOR_MODEL_DIR>` | Overrides detected AI Navigator model path |
| `--workers NUMBER` | Integer, usually 8 or 16 | Parallel file hashing workers |
| `--no-hash` | Nothing | Skips SHA-256 hashing and uses zeroed hashes |
| `--no-symlinks` | Nothing | Updates the DB only and does not create symlinks |
| `--revert` | Nothing | Restores the latest AI Navigator DB backup |
| `--dry-run` | Nothing | Previews actions |

## ***Examples for Prepare_models_for_AIStudio.py***

Use a specific rename plan:
python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json"

Use a specific AI Navigator model folder:
python3 Prepare_models_for_AIStudio.py --target "<AI_NAVIGATOR_MODEL_DIR>"

Use 16 workers:
python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json" --workers 16

Skip hashing:
python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json" --no-hash

Update the database only, without creating symlinks:
python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json" --no-symlinks

Restore the latest database backup:
python3 Prepare_models_for_AIStudio.py --revert

# 🟦 ***4. Prepare_models_for_AnythingLLM.py***

Points AnythingLLM Desktop at the external/system Ollama instead of its bundled Ollama.

Default AnythingLLM env file:
~/Library/Application Support/anythingllm-desktop/storage/.env

Default Ollama URL:
http://localhost:11434

The script also respects:

OLLAMA_HOST

## ***What It Does***

This script can:

- Read the AnythingLLM Desktop `.env` file
- Back up the `.env` file
- Point AnythingLLM at your external/system Ollama instance
- List models visible from Ollama
- Set a preferred default model using substring matching
- Restore the latest `.env` backup

## ***Normal Use***

Preview:
python3 Prepare_models_for_AnythingLLM.py --dry-run

Live run:
python3 Prepare_models_for_AnythingLLM.py

## ***Options for Prepare_models_for_AnythingLLM.py***

| Option | What the user enters | What it does |
|---|---|---|
| `--prefer MODEL` | Part or all of an Ollama model name | Sets a preferred default model |
| `--list-only` | Nothing | Shows models visible from Ollama and exits |
| `--revert` | Nothing | Restores the latest `.env` backup |
| `--dry-run` | Nothing | Previews actions |

## ***Examples for Prepare_models_for_AnythingLLM.py***

List available Ollama models:
python3 Prepare_models_for_AnythingLLM.py --list-only

Prefer a Qwen model:
python3 Prepare_models_for_AnythingLLM.py --prefer "qwen"

Other valid `--prefer` examples:
qwen
llama
mistral
deepseek
qwen2.5-coder

Restore the latest `.env` backup:
python3 Prepare_models_for_AnythingLLM.py --revert

If Ollama is not at `localhost:11434`, set `OLLAMA_HOST` first:
export OLLAMA_HOST="http://localhost:11434"

Then run:

python3 Prepare_models_for_AnythingLLM.py


# 🟦 ***5. Prepare_models_for_GPT4All.py***

Creates symlinks to local GGUF files inside GPT4All’s model folder.

Default GPT4All target is usually:

~/Library/Application Support/nomic.ai/GPT4All

Default source roots should be treated as examples. Users should pass their own GGUF model directories.

## ***What It Does***

This script can:

- Scan one or more roots for `.gguf` files
- Create symlinks in GPT4All’s model folder
- Avoid copying large model files
- Preserve your original model storage layout
- Remove dangling symlinks if `--clean` is used
- Preview actions with `--dry-run`

## ***Normal Use***

Preview:
python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>" --dry-run

Live run:
python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>"

## ***Options for Prepare_models_for_GPT4All.py***

| Option | What the user enters | What it does |
|---|---|---|
| `--gguf-root PATH` | `<GGUF_MODEL_DIR>` | Adds or overrides a GGUF source root |
| `--target PATH` | `<GPT4ALL_MODEL_DIR>` | Overrides GPT4All’s model folder |
| `--workers NUMBER` | Integer, usually 16 | Parallel symlink workers |
| `--clean` | Nothing | Removes dangling symlinks |
| `--dry-run` | Nothing | Previews actions |

## ***Examples for Prepare_models_for_GPT4All.py***

Use a custom GGUF source root:
python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>"

Use multiple GGUF roots:
python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>" --gguf-root "<MODEL_DIR>"

Use a custom GPT4All model folder:
python3 Prepare_models_for_GPT4All.py --target "<GPT4ALL_MODEL_DIR>"

Use 16 workers:
python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>" --workers 16

Clean dangling symlinks:
python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>" --clean


# 🟦 ***6. Prepare_models_for_Jan.py***

Creates symlinks to local GGUF and MLX models inside Jan’s model directories.

Default Jan GGUF target is usually:

~/Library/Application Support/Jan/data/llamacpp/models

Default Jan MLX target is usually:

~/Library/Application Support/Jan/data/mlx/models

Default source roots should be treated as examples. Users should pass their own GGUF and MLX model directories.

## ***What It Does***

This script can:

- Scan for local GGUF files
- Scan for local MLX model directories
- Create symlinks inside Jan’s expected model folders
- Avoid copying large model files
- Skip MLX scanning if requested
- Remove dangling Jan symlinks if `--clean` is used
- Preview actions with `--dry-run`

## ***Normal Use***

Preview:
python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>" --dry-run

Live run:
python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>"

## ***Options for Prepare_models_for_Jan.py***

| Option | What the user enters | What it does |
|---|---|---|
| `--gguf-root PATH` | `<GGUF_MODEL_DIR>` | Adds a custom GGUF source root |
| `--mlx-root PATH` | `<MLX_MODEL_DIR>` | Adds a custom MLX source root |
| `--include-gguf-only` | Nothing | Skips MLX scanning |
| `--workers NUMBER` | Integer, usually 16 | Parallel symlink workers |
| `--clean` | Nothing | Removes dangling Jan symlinks |
| `--dry-run` | Nothing | Previews actions |

## ***Examples for Prepare_models_for_Jan.py***

Use a custom GGUF root:
python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>"

Use a custom MLX root:
python3 Prepare_models_for_Jan.py --mlx-root "<MLX_MODEL_DIR>"

Use both GGUF and MLX roots:
python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>" --mlx-root "<MLX_MODEL_DIR>"

Only prepare GGUF models:
python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>" --include-gguf-only

Use 16 workers:
python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>" --workers 16

Clean dangling symlinks:
python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>" --clean

# 🟣 ***Recommended Run Order***

Use this order for a full setup.

## ***1. LM Studio / Flatten Preparation***

Preview:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>" --dry-run

Live run:

python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>"

## ***2. Ollama Registration***

Preview:

python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>" --dry-run

Live run:

python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>"

## ***3. GPT4All Symlinks***

Preview:

python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>" --dry-run

Live run:

python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>"

## ***4. Jan Symlinks***

Preview:

python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>" --dry-run

Live run:

python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>"

## ***5. AnythingLLM External Ollama Config***

List models first:

python3 Prepare_models_for_AnythingLLM.py --list-only

Then optionally choose a preferred default:

python3 Prepare_models_for_AnythingLLM.py --prefer "qwen"

## ***6. AI Navigator Registration***

Quit AI Navigator first.

Preview:
python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json" --dry-run

Live run:
python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json"

# 🤖 ***Agent Filesystem Wrapper***

This repository also includes an agent filesystem wrapper for local OpenAI-compatible models.

It is designed to help local/open-source models write code to disk using controlled wrapper tools.

The wrapper can:

- Send a task to a local OpenAI-compatible endpoint
- Expose filesystem tools to the model
- Let the model write files under a chosen root folder
- Let the model read files under that root folder
- Let the model list directories
- Optionally run shell commands
- Optionally use local RAG context
- Optionally run CodeQL after file generation

The model itself does not directly touch your disk. The Python wrapper performs the actual local operations.

## ***Required Values***

| Argument | What the user enters |
|---|---|
| `--model` | Exact model name exposed by the local OpenAI-compatible server |
| `--root` | Local project/sandbox folder where the agent can read and write files |
| `--task` | The job you want the model to perform |

## ***Optional Values***

| Argument | What the user enters |
|---|---|
| `--endpoint` | Local server URL, if not `http://localhost:1234/v1` |
| `--api-key` | API key or dummy key required by the local endpoint |
| `--system-prompt` | Path to a custom system prompt file |
| `--rag` | Path to a Markdown or JSONL corpus for retrieval context |
| `--codeql` | Nothing; enables CodeQL if installed |
| `--max-turns` | Integer; increases the number of agent turns |

## ***Minimal Agent Command***

python3 agent_filesystem_wrapper.py --model "MODEL_NAME_HERE" --root "<SANDBOX_DIR>" --task "TASK GOES HERE"

Replace:
MODEL_NAME_HERE

with the model name shown in LM Studio, Ollama, llama-server, or another OpenAI-compatible local server.

Replace:
<SANDBOX_DIR>

with a disposable folder where it is safe for the agent to write files.

Replace:
TASK GOES HERE

with the job you want done.

## ***LM Studio Agent Example***

LM Studio usually exposes an OpenAI-compatible endpoint at:

http://localhost:1234/v1

Example:
python3 agent_filesystem_wrapper.py --model "qwen2.5-coder-7b-instruct" --root "<SANDBOX_DIR>" --task "Create a Python script named hello.py that prints hello world."

If LM Studio uses a different port:
python3 agent_filesystem_wrapper.py --endpoint "http://localhost:1234/v1" --model "MODEL_NAME_HERE" --root "<SANDBOX_DIR>" --task "TASK GOES HERE"

## ***Ollama OpenAI-Compatible Agent Example***

Ollama commonly exposes an OpenAI-compatible endpoint at:
http://localhost:11434/v1

Example:
python3 agent_filesystem_wrapper.py --endpoint "http://localhost:11434/v1" --model "MODEL_NAME_HERE" --root "<SANDBOX_DIR>" --task "Create a Python script named hello.py that prints hello world."

## ***Custom System Prompt***

python3 agent_filesystem_wrapper.py --model "MODEL_NAME_HERE" --root "<SANDBOX_DIR>" --system-prompt "./agent_system_prompt.md" --task "TASK GOES HERE"

## ***RAG Context***

Use a directory or file containing `.md` or `.jsonl` content.

python3 agent_filesystem_wrapper.py --model "MODEL_NAME_HERE" --root "<SANDBOX_DIR>" --rag "./corpus" --task "TASK GOES HERE"

JSONL lines should ideally look like this:
{"text": "relevant context here"}

Markdown files are read directly.

## ***CodeQL***

Use this only if CodeQL CLI is installed and available on PATH.

python3 agent_filesystem_wrapper.py --model "MODEL_NAME_HERE" --root "<SANDBOX_DIR>" --codeql --task "TASK GOES HERE"

# 🔴 ***Agent Wrapper Safety Warning***

Treat `--root` as a disposable workspace.

The wrapper’s direct file tools are root-confined, but shell execution is not a true sandbox. If `run_shell` is enabled, the model may be able to execute commands that access files outside the selected root, depending on operating-system permissions.

For safer use:

- Use a disposable sandbox folder
- Avoid sensitive directories
- Disable `run_shell` if you do not need it
- Run inside a VM or container for high-risk tasks
- Review generated files before executing them

The shell tool is powerful. It can run commands with the selected root as the current working directory, but that does not prevent absolute path access, network commands, environment access, or other normal shell behavior.

# 🛠️ ***Parts Users Might Edit Directly***

Most users should not edit the scripts. Prefer command-line flags.

Only edit code if you want to change defaults or behavior.

## ***Agent Filesystem Wrapper Edit Points***

| Area | Why edit it |
|---|---|
| Default endpoint | Use Ollama or another endpoint by default |
| Default API key | Change the dummy key required by a local server |
| Default system prompt | Make the agent safer or more restrictive |
| `TOOL_SCHEMA` | Remove tools such as `run_shell` |
| `FileTools.run_shell` | Restrict or disable shell execution |
| CodeQL query suite | Use a custom CodeQL query pack |
| `rag_lookup()` | Improve retrieval, chunking, or ranking |

Safer default system prompt example:
You are a coding assistant. Use filesystem tools only when needed. Before running shell commands that modify files, explain the intended command.

Current risky shell behavior to review:
subprocess.run(cmd, shell=True, cwd=self.root, capture_output=True, text=True, timeout=timeout)

This is powerful, but it is not a sandbox.

# 🚫 ***What Users Usually Should Not Modify***

Most users should not need to edit:

- `FENCED_RE`
- `extract_fenced_writes()`
- Main loop structure
- `FileTools._resolve()`
- CodeQL logic unless using custom CodeQL
- `TOOL_SCHEMA` unless changing agent capabilities

# ⚡ ***Quick Reference***

| Script | Required user input | Common command |
|---|---|---|
| `Prepare_models_for_Lmstudio.py` | `<FLAT_MODEL_DIR>` or `<MODEL_DIR>` | `python3 Prepare_models_for_Lmstudio.py --input "<FLAT_MODEL_DIR>"` |
| `Prepare_models_for_Ollama.py` | `<GGUF_MODEL_DIR>` | `python3 Prepare_models_for_Ollama.py --root "<GGUF_MODEL_DIR>"` |
| `Prepare_models_for_AIStudio.py` | `<FLAT_MODEL_DIR>/rename-plan.json` | `python3 Prepare_models_for_AIStudio.py --plan "<FLAT_MODEL_DIR>/rename-plan.json"` |
| `Prepare_models_for_AnythingLLM.py` | Optional preferred model name | `python3 Prepare_models_for_AnythingLLM.py --prefer "qwen"` |
| `Prepare_models_for_GPT4All.py` | `<GGUF_MODEL_DIR>` | `python3 Prepare_models_for_GPT4All.py --gguf-root "<GGUF_MODEL_DIR>"` |
| `Prepare_models_for_Jan.py` | `<GGUF_MODEL_DIR>` and/or `<MLX_MODEL_DIR>` | `python3 Prepare_models_for_Jan.py --gguf-root "<GGUF_MODEL_DIR>"` |
| `agent_filesystem_wrapper.py` | Model, root, task | `python3 agent_filesystem_wrapper.py --model "MODEL_NAME_HERE" --root "<SANDBOX_DIR>" --task "TASK GOES HERE"` |

# 🧾 ***Terminal Copy/Paste Notes***

Use normal double hyphens for options:
--dry-run

Do not use smart dashes:
–dry-run

Use normal straight quotes:
"qwen"

Do not use smart quotes:
“qwen”

On macOS, smart punctuation can be disabled here:

System Settings → Keyboard → Text Input → Edit → turn off “Use smart quotes and dashes”
