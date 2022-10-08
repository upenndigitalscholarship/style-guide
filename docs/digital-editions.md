## Minimal Editions Workflow

This section details our minimal workflow for creating digital editions. Texts and text layers are transformed into static HTML and TEI. Content creation and editing will utilize tools that are familiar and intuitive to our participants. Using natural language processing, the annotations will match their corresponding text in the original Greek and our translations. There are no existing themes for Jekyll or other static site generators designed for the side-by-side presentation of texts, translations, and variants. The templates created for this project can be reused in similar scholarly publications.     

1. Content creation, editing, and review
1. Content pre-planning. Establish what content is needed for the project. This includes translations, commentaries, and types of annotations. Each item is listed in a Google Sheet with title, author, current status, link to Doc, and other relevant fields such as deadlines. This can be updated and changed as needed.
2. A filename standard is created to order individual pages (About, Introduction…) and to relate texts (First Alcibiades) with text layers (Original, Translation, Simplified Translation…) For example, 004-Alcibiades-Greek-Original, 005-Alcibiades-Translation-Jowett. This system should allow for the addition of new texts and text layers over time.
3. Team members use Google Docs for writing, commenting, and editing project content.
4. Translations can also be authored and reviewed in Docs.
5. When ready, the Document is marked for export on the Sheet.


2. Datafication of project content
1. Documents are exported from Drive as HTML using the Drive API. Using HTML retains formatting and layout.
```python
```
2. The HTML files are processed to remove the Drive style block and classes (bs4).
```python 
from bs4 import BeautifulSoup
from pathlib import Path

html = Path('minimal_editions_workflow.html')
soup = BeautifulSoup(html.read_bytes(), "html.parser")

# remove the Drive style block in <head>
soup.head.style.decompose()

# for each element, remove the Drive class attributes
for tag in soup():
    for attribute in ["class"]: 
        del tag[attribute]
html.write_text(str(soup))
```
3. Cleaned HTML files are pushed to a GitHub repository.
4. HTML can be programmatically annotated using a standoff converter; see: [https://so.davidlassner.com/](https://www.google.com/url?q=https://so.davidlassner.com/&sa=D&source=editors&ust=1665080315133338&usg=AOvVaw0BaG4Zjk2I4uNHCdqyYC3R)
5. Use the standoff converter with [Ancient Greek BERT](https://www.google.com/url?q=https://huggingface.co/pranaydeeps/Ancient-Greek-BERT&sa=D&source=editors&ust=1665080315133928&usg=AOvVaw29tOa03MhO9ioT9Pb6Ryg_) to mark clause boundaries and to assign span ids in the HTML(<span id=”123”>text</span>).  This will make it possible to connect spans in translations to similar spans in the originals. We can also add part of speech and morphological analysis data if desired.
6. Commentary can be connected to any span in the HTML (word, phrase, punctuation mark).
7. We could create an application to
1. List the files in the GitHub repository
2. Fetch the HTML and display it in an interface (CKeditor works well with HTML)
3. Allow highlighting of text and addition of commentary
4. Editing and removal of existing annotations
5. Edit the text (given reviewer feedback or other input)
6. Push the updated HTML back to the repository

     3. Publishing

1. Workshop on user experience and site design. Create wireframes and craft website design.
2. From site design, create templates and interface for multi-layer texts.
3. For parallel texts to scroll together, we’ll need to listen for scroll and move both columns of text at the same time. On hover of spans will also need to get elements by id to highlight and display commentary in visible text layers.
4. Use build script to gather the HTML files and create a static site folder.
5. Files can also be converted to TEI for preservation and sharing
