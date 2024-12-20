# MDAgents-MedRefine

This repository builds upon the MDAgents framework for adaptive, multi-agent LLM-based medical decision-making. Our approach introduces prompt-based techniques to:

1. **Medically refine the initial question:** Before processing a medical query, the prompt instructs GPT-4o to rewrite the question into more clinically accurate and professional language.
2. **Enhance inter-agent communication:** Messages between agents are refined to ensure medical terminology and ethical considerations are properly addressed, simulating a "professor" or "cross-communicator" role.

**Note:** This code depends on GPT-4o (or a GPT-4o-like model) accessed via the OpenAI API. We rely on a `.env` file for secure key management. This code is currently experimental and may be costly to run extensively due to the number of prompt calls required per query.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Repository Structure](#repository-structure)
3. [Data Preparation](#data-preparation)
4. [Environment Setup](#environment-setup)
5. [Running the Code](#running-the-code)
6. [Arguments and Configuration](#arguments-and-configuration)
7. [Interpreting the Output](#interpreting-the-output)
8. [Cost and Performance Considerations](#cost-and-performance-considerations)
9. [Troubleshooting](#troubleshooting)
10. [Future Directions](#future-directions)
11. [References](#references)


## Prerequisites

- **Python 3.9+** recommended.
- An [OpenAI API key](https://platform.openai.com/) to access GPT-4o-like endpoints.
- Basic familiarity with Python, virtual environments, and handling `.env` files.

## Repository Structure

```
MDAgents-MedRefine/
├── main.py
├── utils.py
├── requirements.txt
├── .env
└── data/
    └── medqa/
        ├── train.jsonl
        └── test.jsonl
```

**Key Files:**

- **main.py:** Entry point script that processes dataset queries, refines the question, determines complexity, and runs multi-agent decision-making.
- **utils.py:** Contains helper classes and functions (e.g., `Agent`, data loading, message refinement, MDAgents logic).
- **.env:** Stores secrets like `openai_api_key`.
- **requirements.txt:** Lists required Python packages.

## Data Preparation

1. Place your datasets in `./data/{dataset}/`.  
   For example, if using `medqa`, put:
   - `train.jsonl` at `./data/medqa/train.jsonl`
   - `test.jsonl` at `./data/medqa/test.jsonl`

2. Ensure `train.jsonl` and `test.jsonl` follow the expected format:
   Each line is a JSON object representing a question, possible answers, and additional metadata (like the correct answer).

## Environment Setup

1. **Create a virtual environment:**
   ```bash
   conda create -n mdagents-medrefine python=3.9
   ```

2. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up `.env`:**
   Create a `.env` file in the project root if you haven’t already. Add:
   ```
   openai_api_key=YOUR_OPENAI_API_KEY
   ```
   
   Replace `YOUR_OPENAI_API_KEY` with your actual OpenAI API key.

## Running the Code

After the environment and data are ready, run:

```bash
python main.py --dataset medqa --model gpt-4o-mini --difficulty adaptive --num_samples 10
```

### What This Command Does

- `--dataset medqa`: Uses the `medqa` dataset located under `./data/medqa/`.
- `--model gpt-4o-mini`: Uses GPT-4o or a model named similarly.
- `--difficulty adaptive`: Automatically determines the complexity for each question (basic, intermediate, advanced).
- `--num_samples 10`: Process only 10 samples as a test.

## Arguments and Configuration

- `--dataset {name}`: Name of the dataset (folder under `./data/`).
- `--model {model_name}`: Model identifier (e.g., `gpt-4o-mini`).
- `--difficulty {basic|intermediate|advanced|adaptive}`: If `adaptive`, complexity is chosen dynamically.
- `--num_samples N`: Limit the number of processed questions for a quick test.

## Interpreting the Output

1. The code prints logs:
   - Complexity determination for each question.
   - Steps of agent collaboration.
   - The final decision-making process.

2. Final results are saved as JSON in `./output/{model}_{dataset}_{difficulty}.json`.  
   Each entry includes:
   - The medically refined question.
   - The chosen complexity level.
   - The model’s final decision and the reasoning steps.

## Cost and Performance Considerations

- **Multiple API Calls:**  
  Each question involves several calls: initial question refinement, complexity determination, agent communications refinements.
- **Financial Constraints:**  
  Start with low `--num_samples` to keep costs manageable.
- **Caching (Future Work):**  
  Consider caching responses or simplifying prompts if you aim to scale.

## Troubleshooting

- **No Output/Empty Results:**  
  Check that `train.jsonl` and `test.jsonl` files are correctly formatted and in the correct directory.
- **Authentication Errors:**  
  Confirm that `.env` is present and `openai_api_key` is set correctly.
- **Rate Limits or Model Access Issues:**  
  Ensure you have the correct OpenAI plan or model access.

## Future Directions

- **Integration with Specialized Medical LLMs:**  
  If domain-specific LLMs become accessible, we can reduce prompt complexity.
- **Efficiency and Caching:**  
  Implement caching to reduce repeated prompts and cut down on costs.
- **Extended Benchmarks:**  
  Once resources allow, run on more samples or different datasets for comprehensive evaluation.

## References

This work builds upon the MDAgents framework:

- **MDAgents:**
  - [MDAgents: An Adaptive Collaboration of LLMs for Medical Decision-Making](https://arxiv.org/abs/2404.15155)  
    *Kim, Yubin, et al. "MDAgents: An Adaptive Collaboration of LLMs for Medical Decision-Making." arXiv preprint arXiv:2404.15155 (2024).*
  
  - [A Demonstration of Adaptive Collaboration of Large Language Models for Medical Decision-Making](https://arxiv.org/abs/2411.00248)  
    *Kim, Yubin, et al. "A Demonstration of Adaptive Collaboration of Large Language Models for Medical Decision-Making." arXiv preprint arXiv:2411.00248 (2024).*

These papers introduced the adaptive multi-agent approach that our project extends through prompt-based refinement techniques.
