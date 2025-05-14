# pdf
!pip install fpdf python-docx pillow
from fpdf import FPDF
from PIL import Image
import os
from docx import Document
from google.colab import files

def convert_to_pdf(input_path):
    ext = os.path.splitext(input_path)[1].lower()
    pdf = FPDF()
    output_path = input_path.rsplit(".", 1)[0] + ".pdf"

    if ext == ".txt":
        pdf.add_page()
        pdf.set_font("Arial", size=12)
        with open(input_path, "r", encoding="utf-8") as file:
            pdf.multi_cell(190, 10, file.read())

    elif ext in [".jpg", ".jpeg", ".png"]:
        pdf.add_page()
        pdf.image(input_path, x=10, y=10, w=pdf.w - 20)

    elif ext == ".docx":
        pdf.add_page()
        pdf.set_font("Arial", size=12)
        doc = Document(input_path)
        for para in doc.paragraphs:
            pdf.multi_cell(190, 10, para.text)

    else:
        print("Unsupported file format!")
        return

    pdf.output(output_path)
    print(f"Converted to PDF: {output_path}")
    files.download(output_path)

uploaded = files.upload()
convert_to_pdf(list(uploaded.keys())[0])
