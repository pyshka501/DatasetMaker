# Dataset Generation System with g4f

A robust, scalable, and production-ready dataset generation framework in Python for creating synthetic datasets using the g4f library (OpenAI-compatible API). The system is highly configurable via YAML, supports parallel generation, and is built with clean OOP architecture. Prompts and configuration are separated into dedicated files for easy editing and versioning.

## Features

- **Parallel Generation**: Run N generators in parallel (configurable).
- **Configurable via YAML**: Easily adjust all parameters (model, number of generators, retries, paths, etc.).
- **Prompt Management**: System and user prompts are stored in separate text files.
- **Resilience**: Automatic timeouts, retries, and error handling for unstable g4f connections.
- **Clear File Structure**: Individual generator outputs, logs, configs, and final dataset are organized in a transparent directory structure.
- **OOP Design**: All logic is encapsulated in classes for maintainability and extensibility.
- **Deduplication**: Final dataset contains only unique examples.
- **Easy to Run**: Launch the entire pipeline with a single script.

## Directory Structure

```
project_root/
├── config.yaml
├── prompts/
│   ├── system_prompt.txt
│   └── user_prompt.txt
├── dataset_output/
│   ├── logs/
│   ├── generators/
│   └── final_dataset.csv
├── main.py
├── requirements.txt
└── ...
```

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/g4f-dataset-generator.git
cd g4f-dataset-generator
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Prepare Configuration

Edit `config.yaml` to set your desired parameters:

```yaml
model_name: gpt-4o-mini
num_generators: 8
generations_per_entity: 20
timeout_seconds: 15
max_retries: 100
output_dir: dataset_output
csv_column_name: examples
system_prompt_path: prompts/system_prompt.txt
user_prompt_path: prompts/user_prompt.txt
```

### 4. Write Prompts

Edit `prompts/system_prompt.txt` and `prompts/user_prompt.txt` with your desired instructions.

### 5. Run the Generator

```bash
python main.py
```

The system will generate N mini-datasets in parallel, then merge them into `dataset_output/final_dataset.csv` (duplicates removed).

## Configuration

All settings are managed in `config.yaml`:

| Parameter              | Description                                      | Example          |
|------------------------|--------------------------------------------------|------------------|
| `model_name`           | g4f model name                                   | gpt-4o-mini      |
| `num_generators`       | Number of parallel generators                    | 8                |
| `generations_per_entity` | Number of generations per generator            | 20               |
| `timeout_seconds`      | Timeout for each API call (seconds)              | 15               |
| `max_retries`          | Max retries per generation                       | 100              |
| `output_dir`           | Output directory for results                     | dataset_output   |
| `csv_column_name`      | Name of the column in CSV files                  | examples         |
| `system_prompt_path`   | Path to system prompt file                       | prompts/system_prompt.txt |
| `user_prompt_path`     | Path to user prompt file                         | prompts/user_prompt.txt   |

## Usage Example

```python
from main import GeneratorConfig, DatasetManager

config = GeneratorConfig('config.yaml')
manager = DatasetManager(config)
manager.run_all()
manager.merge_results()
```

## File Structure

- **config.yaml**: Main configuration file (YAML).
- **prompts/**: Folder for prompt text files.
- **dataset_output/generators/**: Individual CSVs from each generator.
- **dataset_output/final_dataset.csv**: Combined, deduplicated dataset.
- **main.py**: Entry point for the generation pipeline.

## Error Handling & Stability

- Each API call is run in a separate process with a timeout.
- Automatic retries with exponential backoff on failure.
- All intermediate and final results are saved atomically.
- Logging for all errors and progress.

## Extending & Customization

- Add new prompt files for different tasks.
- Adjust YAML config for different models or output formats.
- Expand classes for new generation logic or post-processing.

## Requirements

- Python 3.8+
- g4f >= 0.2.7
- PyYAML >= 6.0
- pandas >= 2.0.0

Install all dependencies via:

```bash
pip install -r requirements.txt
```

## License

MIT License

## Authors

Pchelin Konstantin

## FAQ

**Q:** How do I change the model or prompts?  
**A:** Edit `config.yaml` for the model, and the text files in `prompts/` for the prompts.

**Q:** Can I run more generators in parallel?  
**A:** Yes, set `num_generators` in `config.yaml` according to your CPU and needs.

**Q:** Where is the final dataset?  
**A:** After execution, see `dataset_output/final_dataset.csv`.

For any issues, please open an issue or pull request on GitHub.
