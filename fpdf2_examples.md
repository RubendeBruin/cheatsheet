- Multiline table
- header, footer

```python
from fpdf import FPDF, HTMLMixin

class PDF(FPDF, HTMLMixin):
    def header(self):
        # Logo
        self.image(r"C:\data\Dave\Images\Logo\Dave logo normal.png", y = 10, x=190, w=10, h=10)
        # helvetica bold 15
        self.set_font('helvetica', 'B', 15)
        # Move to the right
        self.cell(80)
        # Title
        self.cell(30, 10, 'Title', 1, 0, 'C')
        # Line break
        self.ln(20)

    # Page footer
    def footer(self):
        # Position at 1.5 cm from bottom
        self.set_y(-15)
        # helvetica italic 8
        self.set_font('helvetica', 'I', 8)
        # Page number
        self.cell(0, 10, 'Page ' + str(self.page_no()) + '/{nb}', 0, 0, 'C')

# Instantiation of inherited class
pdf = PDF()
# pdf.alias_nb_pages(2)
pdf.add_page()
pdf.set_font('Times', '', 12)
data = (
    ("First name", "Last name", "Age", "City"),
    ("Jules", "Smith", "34", "San Juan"),
    ("Mary", "Ramos\n \n Ramos \n    Ramos", "45", "Orlando"),
    ("Lucas", "Cimon", "Saint-Mahturin-sur-Loire - it may even be so long that multiple lines are needed to write it down completely", "49"),
    ("Carlson", "Banks", "19", "Los Angeles"),
)

pdf.add_page()
line_height = pdf.font_size * 1.1
col_width = pdf.epw /6   # distribute content evenly
for row in data:

    row_height_lines = 1
    lines_in_row = []
    for datum in row:
        output = pdf.multi_cell(col_width, line_height, datum, border=1, ln=3, split_only=True)
        lines_in_row.append(len(output))
        if len(output) > row_height_lines:
            row_height_lines = len(output)

    for tlines , datum in zip(lines_in_row, row):
        # here you can hack-in the
        text =datum.rstrip('\n') + (1 + row_height_lines - tlines) * '\n'
        pdf.multi_cell(col_width, line_height, text, border=1, ln=3)
    pdf.ln(row_height_lines * line_height)


pdf.output(r'c:\data\temp\tuto2.pdf')
```
