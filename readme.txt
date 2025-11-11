# README

## Overview
This bundle contains the notebooks (recipes), prompts, and prompt outputs used in the study on extracting structured **data-wrangling metadata** from static code using **Gemini 2.0 Flash**. The workflow reads a notebook, extracts code cells, and sends them to Gemini along with a task prompt. Gemini returns a **single TSV** summary table with eleven fixed rows. Table-level accuracy is computed against a ground-truth table (prepared by manual code reading) as described in the report.

> **Note on Dataset 5 / Recipe 1:** The file is an **R script** (`Dataset 5 Recipe 1.R`). The API script targets `.ipynb` notebooks and does not process R scripts. **This one recipe was run in the Gemini online interface.** Its summary table is included in *Prompt_Outputs/Dataset 5.pdf* alongside the other Dataset 5 results.

## Folder layout
```
CSDI-14103061-am/
├─ API_prompt.ipynb
├─ Datasets with Recipes/
│  ├─ Dataset 1 and Recipes/
│  │  ├─ Dataset 1 Recipe 1.ipynb ... Dataset 1 Recipe 10.ipynb
│  │  └─ Dataset 1.csv
│  ├─ Dataset 2 and Recipes/
│  │  ├─ Dataset 2 Recipe 1.ipynb ... Dataset 2 Recipe 10.ipynb
│  │  └─ *.csv (dataset files)
│  ├─ Dataset 3 and Recipes/
│  │  ├─ Dataset 3 Recipe 1.ipynb ... Dataset 3 Recipe 10.ipynb
│  │  ├─ train.csv, test.csv
│  │  └─ *.csv (dataset files)
│  ├─ Dataset 4 and Recipes/
│  │  ├─ Dataset 4 Recipe 1.ipynb ... Dataset 4 Recipe 10.ipynb
│  │  └─ Real estate.csv
│  └─ Dataset 5 and Recipes/
│     ├─ Dataset 5 Recipe 1.R    ← R script (handled via Gemini web UI)
│     └─ Dataset 5 Recipe 2.ipynb ... Dataset 5 Recipe 10.ipynb
├─ Prompts/
│  ├─ FirstAttempt_ExtractingFromPythonScriptsDWTechniquesUsingChatGPT.docx
│  ├─ SecondAttempt_ExtractingFromPythonScriptsDWTechniquesUsingChatGPT.docx
│  ├─ ThirdAttempt_ExtractingFromPythonScriptsDWTechniquesUsingChatGPT.docx
│  └─ Redesigned_Prompts.docx
└─ Prompt_Outputs/
   ├─ Dataset 1.pdf
   ├─ Dataset 2.pdf
   ├─ Dataset 3.pdf
   ├─ Dataset 4.pdf
   └─ Dataset 5.pdf
```

*Note:* The *Prompt_Outputs* PDFs collect the summary tables produced by Gemini per dataset.

## How to run the Gemini API notebook

1. **Set your Google AI key** in your environment before launching Jupyter:
   - macOS / Linux:
     ```bash
     export GOOGLE_API_KEY="YOUR_KEY"
     ```
   - Windows PowerShell:
     ```powershell
     setx GOOGLE_API_KEY "YOUR_KEY"
     ```

2. **Open** `API_prompt.ipynb` in Jupyter (VS Code, JupyterLab, or classic notebook). The notebook contains Python code that:
   - extracts code cells from a target `.ipynb` recipe,
   - concatenates them into a single prompt with your task text,
   - sends the prompt to **gemini-2.0-flash** using the `google-genai` SDK, and
   - prints Gemini's TSV summary table.

3. **Set paths and prompt** inside the notebook:
   - `my_notebook` → path to the target recipe, e.g.  
     `./Datasets with Recipes/Dataset 1 and Recipes/Dataset 1 Recipe 1.ipynb`
   - `my_prompt` → paste the text of one of your prompts from `Prompts/Redesigned_Prompts.docx` (Redesign‑1 or Redesign‑2) or a seed prompt from the other DOCX files.

4. **Run the notebook** to print the TSV. Save the TSV to disk if you wish. (In the study, each TSV was scored out of 11 with the accuracy printed at the top of the file.)

### Dependencies
- Python 3.10+
- `google-genai` (new Google AI Python SDK)
  ```bash
  pip install google-genai
  ```

## Notes and caveats
- **Input format:** The script reads **code cells** from `.ipynb`. It does **not** execute notebooks and does **not** fetch data.
- **R notebooks/scripts:** The API notebook does not process `.R` or `.Rmd`. For `Dataset 5 Recipe 1.R`, results were generated using the **Gemini online interface** and are included in *Prompt_Outputs/Dataset 5.pdf*.
- **Prompt discipline:** Use the redesigned prompts to enforce a single **tab‑separated** table with **no extra text** and a **fixed row order**.
- **Reproducibility:** Keep copies of Gemini outputs (TSVs or PDFs) **and** the ground‑truth tables for each recipe. Cross‑reference them in your report appendices.

## Suggested outputs organisation (optional)
If you plan to regenerate TSVs, consider adding an `outputs/` folder with subfolders per prompt and per dataset, e.g.:
```
outputs/
  Redesign_1/
    dataset_1/recipe_01.tsv ... recipe_10.tsv
    ...
  Redesign_2/
    dataset_1/...
```
This mirrors the evaluation pipeline described in the report and makes it easy to recompute dataset and prompt totals.