# PDF to Image Conversion and AWS Textract Analysis

A Python utility for converting PDF documents to images and analyzing expense documents using AWS Textract's AnalyzeExpense API.

## Prerequisites

- Python
- AWS Account with appropriate permissions
- AWS credentials (Access Key ID and Secret Access Key)
- S3 bucket for document storage

## Installation

```bash
# Install required packages
pip install pdf2image
pip install PyMuPDF
pip install amazon-textract-textractor
```

## Part 1: PDF to Image Conversion

### Required Libraries
```python
import os
import fitz  # PyMuPDF
```

### Usage
```python
# Configure paths
pdf_file = "<File Path of your pdf document>"
output_dir = "<File path to save the image of your pdf document>"

# Convert PDF to images
pdf_to_images(pdf_file, output_dir)
```

### Function Details
```python
def pdf_to_images(pdf_file, output_dir):
    """
    Converts a PDF file to multiple PNG images (one per page).
    
    Parameters:
        pdf_file (str): Path to the input PDF file
        output_dir (str): Directory where the output images will be saved
        
    Output:
        Creates PNG files named 'page_1.png', 'page_2.png', etc. in the output directory
    """
```

## Part 2: AWS Textract Analysis

### Required Libraries
```python
from textractor import Textractor
from textractor.data.constants import (
    AnalyzeExpenseFields, 
    AnalyzeExpenseFieldsGroup, 
    AnalyzeExpenseLineItemFields
)
```

### AWS Configuration
```python
# Set AWS credentials
os.environ["AWS_ACCESS_KEY_ID"] = "<Your AWS Access Key ID>"
os.environ["AWS_SECRET_ACCESS_KEY"] = "<Your AWS Secret Access Key>"

# Initialize Textractor
extractor = Textractor(region_name="ap-southeast-1")  # Replace with your AWS region
```

### Analyzing Expenses
```python
# Analyze expense document
document = extractor.analyze_expense(
    file_source="<path_to_image>",
    save_image=True
)

# Visualize results
document.visualize(with_words=False)
```

### Accessing Analysis Results

1. Access expense document:
```python
expense_doc = document.expense_documents[0]
```

2. View available data:
```python
# View summary fields
expense_doc.summary_fields

# View summary groups
expense_doc.summary_groups

# View line items groups
expense_doc.line_items_groups

# Convert line items to pandas DataFrame
expense_doc.line_items_groups[0].to_pandas()
```

## Output Data Structure

The analysis provides:
- Summary fields (key-value pairs from the document)
- Summary groups (grouped related fields)
- Line item groups (tabular data from the document)
- Metadata including:
  - Page count
  - Word count
  - Line count
  - Number of tables
  - Number of forms
  - Number of key-value pairs

## Important Notes

1. **AWS Textract Limitations**:
   - Only works with images and documents stored in S3 buckets
   - Region-specific availability
   - Requires proper AWS permissions

2. **PDF Conversion**:
   - Creates separate PNG files for each page
   - Maintains original resolution
   - Requires sufficient disk space for output files
