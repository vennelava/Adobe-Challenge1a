
# PDF Outline Extractor

## Overview

This project was developed as a solution for Adobe Challenge 1A: "Connecting the Dots Through Docs". The goal is to enable machines to better interpret the structure of documents by extracting a meaningful outline from PDFs. The solution identifies the title and hierarchical headings (H1, H2, H3) along with their page numbers, and outputs this information in a clean, structured JSON format.

## Key Features

- Processes PDF files (up to 50 pages)
- Automatically detects:
  - Title
  - Headings (H1, H2, H3) with associated levels and page numbers
- Outputs are saved in JSON format, adhering to a defined schema
- Operates completely offline without internet access
- Compatible with Docker (amd64 CPU architecture)

## Output Format

Each PDF generates a corresponding `.json` file with the following structure:

```json
{
  "title": "Understanding AI",
  "outline": [
    { "level": "H1", "text": "Introduction", "page": 1 },
    { "level": "H2", "text": "What is AI?", "page": 2 },
    { "level": "H3", "text": "History of AI", "page": 3 }
  ]
}
```

## Methodology

This solution uses `PyMuPDF (fitz)` to extract structural data from PDFs. The process involves:
- Parsing all text spans, font sizes, and layout metadata
- Identifying potential headings based on font size, boldness, and placement
- Assigning heading levels dynamically relative to the body text size
- Filtering out repeated elements to reduce noise
- Deriving the document title based on the largest text element on the first page

## Project Structure

```
.
├── sample_dataset/
│   ├── pdfs/                   # Input PDF files
│   ├── outputs/                # Output JSON files
│   └── schema/
│       └── output_schema.json  # Output format reference
├── dockerfile                  # Docker configuration
├── process_pdfs.py             # Core processing script
├── requirements.txt            # Python package requirements
└── README.md                   # Project documentation
```

## How to Run

### With Docker

To build the Docker image:

```bash
docker build --platform linux/amd64 -t pdf-outline-extractor:latest .
```

To run the container:

```bash
docker run --rm \
  -v $(pwd)/sample_dataset/pdfs:/app/input \
  -v $(pwd)/sample_dataset/outputs:/app/output \
  --network none \
  pdf-outline-extractor:latest
```

### Without Docker (for local development)

Install the required packages:

```bash
pip install -r requirements.txt
```

Run the script manually:

```bash
python process_pdfs.py
```

This processes all `.pdf` files from `sample_dataset/pdfs` and writes output `.json` files to `sample_dataset/outputs`.

## Constraints Addressed

- Execution time within 10 seconds for a 50-page PDF
- Fully offline functionality
- No network or API calls
- Compatible with systems using amd64 architecture
- No GPU or external model dependencies
