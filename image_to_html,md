Snippet:

```python
from PIL import Image
import base64
from io import BytesIO


image = Image.open(r'c:\data\test.png')

outputBuffer = BytesIO()
image.save(outputBuffer, format='PNG')
bgBase64Data = outputBuffer.getvalue()

outputBuffer.seek(0)
data_uri = base64.b64encode(outputBuffer.read()).decode('ascii')

html = '<html><head></head><body>'
html += '<img src="data:image/png;base64,{0}">'.format(data_uri)
html += '</body></html>'

print (html)

with open(r'c:\data\test.html', 'w') as f:
    f.write(html)

```
