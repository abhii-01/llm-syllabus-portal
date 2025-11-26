# PDF Taxonomy Tagger

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/abhii-01/llm-syllabus-portal/blob/sample-check/pdf_taxonomy_tagger.ipynb)

A system for automatically tagging PDF paragraphs with hierarchical taxonomy paths using LLM-based semantic matching.

## Overview

This tool extracts paragraphs from PDF files and matches them against a hierarchical taxonomy structure. Each paragraph is tagged with its position in the taxonomy (as deep as confidence allows), enabling:

- **Partial matching**: Paragraphs matched only to confident taxonomy levels
- **New branch tracking**: Identifies content that doesn't fit existing taxonomy
- **Quality assessment**: Shows how well your PDFs align with the taxonomy
- **Vector DB preparation**: Aggregates content by topic for embedding

## Features

- ✅ Extract paragraphs from PDFs with page numbers
- ✅ Hierarchical LLM-based matching with confidence scores
- ✅ Stops matching when confidence threshold not met (partial paths)
- ✅ Tracks new branches for quality assessment
- ✅ Batch processing for multiple PDFs
- ✅ Topic-level aggregation for vector database creation
- ✅ Modular design for easy experimentation
- ✅ Google Colab ready with multiple API key options

## Quick Start

### 1. Setup

Copy the secrets template and add your API key:

```bash
cp secrets_template.json secrets.json
# Edit secrets.json and add your OpenAI or Anthropic API key
```

### 2. Run the Notebook

Open `pdf_taxonomy_tagger.ipynb` in Jupyter or Google Colab and run all cells to set up.

### 3. Process a PDF

**Option A: Quick Start (Recommended)**

Go to Cell #10 and edit the PDF path:

```python
pdf_path = '/content/drive/MyDrive/economics1.pdf'  # ← Change this to your PDF path
```

Then run the cell - it automatically:
- Mounts Google Drive (if needed)
- Validates the file
- Processes the PDF
- Downloads results

**Option B: Interactive Mode**

Use Cell #11 if you prefer to be prompted for the path.

### 4. View Results

Results are saved to `output/{pdf_name}_tagged.json`. Each paragraph record looks like:

```json
{
  "paragraph_id": "economics_001",
  "text": "Planning is an essential process...",
  "taxonomy_path": ["ECONOMIC DEVELOPMENT", "Planning", "Meaning of Planning"],
  "match_depth": 3,
  "is_new_branch": false,
  "metadata": {
    "pdf_file": "economics.pdf",
    "page_number": 5,
    "paragraph_number": 1
  }
}
```

## Files

- `pdf_taxonomy_tagger.ipynb` - Main Jupyter notebook
- `taxonomy_gs3.json` - Hierarchical taxonomy structure
- `config_template.json` - Configuration options
- `secrets_template.json` - API key template
- `.gitignore` - Excludes secrets and PDFs from git

## Configuration

Edit `config_template.json` or modify `MATCHING_CONFIG` in the notebook:

```python
MATCHING_CONFIG = {
    "threshold": 70,         # Minimum score to accept a match
    "min_score_diff": 10,    # Minimum gap between top and second match
    "llm_model": "gpt-4",    # OpenAI or Anthropic model
    "temperature": 0.3,      # Lower = more deterministic
    "max_retries": 3,
    "debug_mode": True       # Print detailed logs
}
```

## Prompt Customization

Easily experiment with different prompts by editing `PROMPTS` dictionary:

```python
PROMPTS = {
    "system": "You are an expert at categorizing...",
    "matching": "Analyze this paragraph...",
    "matching_short": "Rate how well..."
}
```

## Partial Matching Example

**Paragraph**: "This discusses a new economic concept not in the taxonomy."

**Output**:
```json
{
  "taxonomy_path": ["ECONOMIC DEVELOPMENT"],
  "match_depth": 1,
  "is_new_branch": true
}
```

The system matched confidently to "ECONOMIC DEVELOPMENT" but couldn't match any subtopic, so it stopped there. This helps track content that might need new taxonomy categories.

## Batch Processing

Process multiple PDFs and aggregate by topic:

```python
pdf_paths = ["pdf1.pdf", "pdf2.pdf", "pdf3.pdf"]
all_results = process_multiple_pdfs(pdf_paths, MATCHING_CONFIG, TAXONOMY)
topic_data = aggregate_by_topic(all_results)
```

Creates files like:
- `output/ECONOMIC_DEVELOPMENT_all_paragraphs.json`
- `output/AGRICULTURE_all_paragraphs.json`
- etc.

Perfect for creating topic-specific vector databases!

## Google Colab Usage

### 🚀 **Instant Start (One Click)**

Use this link to open directly in Colab:

```
https://colab.research.google.com/github/abhii-01/llm-syllabus-portal/blob/sample-check/pdf_taxonomy_tagger.ipynb
```

### 🔑 **Setup API Key**

The notebook automatically detects Colab and provides three API key options:

1. **Colab Secrets** (Recommended): 
   - Click the 🔑 key icon in Colab sidebar
   - Add secret: `OPENAI_API_KEY` = `sk-your-key`
   - Toggle "Notebook access" ON

2. **Google Drive**: Save `secrets.json` in your Drive at `/MyDrive/secrets.json`

3. **Direct Input**: Enter key when prompted

### 📄 **Setup Your PDF**

**Option 1: Use Google Drive (Recommended)**
- Upload PDF to your Google Drive
- In Cell #10, set: `pdf_path = '/content/drive/MyDrive/your_file.pdf'`
- Run the cell - Drive mounts automatically!

**Option 2: Upload Directly**
- In Cell #10, uncomment the upload lines:
```python
from google.colab import files
uploaded = files.upload()
pdf_path = list(uploaded.keys())[0]
```

Then just run Cell #10 and you're done! 🎉

## Output Structure

### Single PDF Output

`output/{pdf_name}_tagged.json` contains all paragraphs with their matches.

### Topic Aggregation

`output/{TOPIC_NAME}_all_paragraphs.json` contains all paragraphs from all PDFs that matched to that topic.

### Statistics

The system prints:
- Match rate (fully matched vs new branches)
- Depth distribution (how deep matches went)
- Topic distribution (for aggregated results)

## Troubleshooting

**Issue**: "taxonomy_gs3.json not found"
- Make sure the file is in the same directory as the notebook
- In Colab, upload it or clone the repo

**Issue**: API errors
- Check your API key in `secrets.json`
- Verify you have credits/quota remaining
- Try increasing `max_retries` in config

**Issue**: Low match rates
- Lower the `threshold` (try 60 instead of 70)
- Set `min_score_diff` to 0 to accept closer matches
- Review prompts - might need adjustment for your content

## Next Steps

After processing PDFs:

1. **Review new branches**: Check paragraphs with `is_new_branch: true`
2. **Adjust taxonomy**: Add new categories if needed (in `taxonomy_gs3.json`)
3. **Create embeddings**: Use topic-aggregated JSONs with your embedding model
4. **Build vector DB**: Import embedded paragraphs into your vector database

## License

This project is for educational/research purposes.

