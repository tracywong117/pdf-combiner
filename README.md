# PDF Combiner
This Python script allows you to combine multiple PDF files into a single PDF using the PyPDF2 library.

## Features
- Given a list of PDFs, the script combines them in the same order as the list.
- Each PDF title is added to the **outline** of the combined PDF.

```Python
from PyPDF2 import PdfMerger, PdfReader

def combine_pdfs(output_path, index_list, parse_title_fn=None):
    merger = PdfMerger()

    page_num = 0

    for index, pdf_filename in enumerate(index_list, start=1):
        print(f"Adding {pdf_filename}")

        input_pdf = PdfReader(pdf_filename)
        input_pdf_title = pdf_filename.split('.')[0]
        input_pdf_title = parse_title_fn(input_pdf_title) if parse_title_fn else input_pdf_title
        print(input_pdf_title)

        merger.append(pdf_filename)
        merger.add_outline_item(input_pdf_title, page_num, None) # add outline
        page_num += len(input_pdf.pages)
    
    # Write the combined PDF to the output path
    merger.write(output_path)

    merger.close()

# Example usage
combine_pdfs("combined.pdf", ["a.pdf", "b.pdf"])
```
