# Read Aloud Examples

> This document is a work in progress meant to explore some of the common use cases that a read aloud implementation based on Guided Navigation will have to deal with.

## Sections and headers

### HTML

```html
<section role="doc-chapter" epub:type="chapter">
  <h1>Title of the chapter</h1>
</section>
```

### JSON

```json
{
  "role": ["section", "chapter"],
  "children": [
    {
      "role": ["heading"],
      "level": 1,
      "text": "Title of the chapter"
    }
  ]
}
```

## Paragraphs

### HTML

```html
<p>It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife.</p>
```

### JSON

```json
{
  "role": ["paragraph"],
  "text": "It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife."
}
```

## Multilingual text

### HTML

```html
<p>This job requires a certain <em xml:lang="fr">savoir faire</em> that can only be acquired over time.</p>
```

### JSON

**Using SSML**

```json
{
  "role": ["paragraph"],
  "text": {
    "ssml": "<speak xml:lang=\"en\">This job requires a certain <voice xml:lang=\"fr\">savoir faire</voice> that can only be acquired over time.</speak>"
  }
}
```

**Using multiple text elements**

```json
{
  "role": ["paragraph"],
  "children": [
    {
      "text": {
        "plain": "This job requires a certain",
        "language": "en"
      }
    },
    {
      "text": {
        "plain": "savoir faire",
        "language": "fr"
      }
    },
    {
      "text": {
        "plain": "that can only be acquired over time.",
        "language": "en"
      }
    }
  ]
}
```

## Images

### HTML

```html
<img src="image1.avif" alt="Alternative text using the alt attribute">
<span role="img" aria-label="Rating: 4 out of 5 stars">
  <span>★</span>
  <span>★</span>
  <span>★</span>
  <span>★</span>
  <span>☆</span>
</span>
<figure aria-labelledby="cat-caption"> 
  <pre>
 /\_/\
( o.o )
   ^ 
  </pre>
 <figcaption id="cat-caption">
  ASCII Art of a cat face
 </figcaption>
</figure>
```

### JSON

```json
{
  "guided": [
    {
      "role": ["image"],
      "imgref": "image1.avif",
      "description": "Alternative text using the alt attribute"
    },
    {
      "role": ["image"],
      "description": "Rating: 4 out of 5 stars"
    },
    {
      "role": ["figure"],
      "description": "ASCII Art of a cat face"
    }
  ]
}
```

## Pagebreaks

> This is an experimental approach based on templated text where `{pagebreak}` can be substituted by the reading system based on user preferences.

### HTML

```html
<span id="pg04" role="doc-pagebreak" epub:type="pagebreak" title="4"/>
<p>And the next pagebreak is in the middle <span id="pg05" role="doc-pagebreak" epub:type="pagebreak" title="4"/> of a sentence.</p>
```

### JSON

```json
{
  "guided": [
    {
      "role": ["pagebreak"],
      "text": "4"
    },
    {
      "role": ["paragraph"],
      "text": "And the next pagebreak is in the middle {pagebreak} of a sentence.",
      "children": [
        {
          "role": ["pagebreak"],
          "text": "5"
        }
      ]
    }
  ]
}
```

### Read-aloud output

**User preference set to read pagebreaks at the end of the text**

* Pagebreak. Page: 4.
* And the next pagebreak is in the middle of a sentence.
* Pagebreak. Page: 5.

**User preference set to read pagebreaks in the middle of the text**

* Pagebreak. Page: 4.
* And the next pagebreak is in the middle (Pagebreak. Page: 5.) of a sentence.

**User preference set to skip pagebreaks**

* And the next pagebreak is in the middle of a sentence.

## Lists

### HTML

```html
<ul>
  <li>First item</li>
  <li>Second item</li>
  <li>Third item</li>
</ul>
```

### JSON

```json
{
  "role": ["list"],
  "children": [
    {
      "role": ["listItem"],
      "text": "First item"
    },
    {
      "role": ["listItem"],
      "text": "Second item"
    },
    {
      "role": ["listItem"],
      "text": "Third item"
    }
  ]
}
```

## Footnotes and endnotes

> This is an experimental approach based on templated text where `{noteref}` can be substituted by the reading system based on user preferences.

### HTML

```html
<p>This text has a footnote in the same resource <a href="#note1" epub:type="noteref" role="doc-noteref">[1]</a> and an endnote <a href="endnote.xhtml#note2" epub:type="noteref" role="doc-noteref">[2]</a>.</p>
<p>This is a paragraph without a footnote.</p>
<aside id="note1" epub:type="footnote" role="doc-footnote">Text of the footnote</aside>
```

### JSON

```json
{
  "guided": [
    {
      "role": ["paragraph"],
      "text": "This text has a footnote in the same resource {noteref} and an endnote {noteref}.",
      "children": [
        {
          "role": ["noteref"],
          "text": "[1]",
          "children": [
            {
              "role": ["footnote", "aside"],
              "text": "Text of the footnote"
            }
          ]
        },
        {
          "role": ["noteref"],
          "text": "[2]",
          "children": [
            {
              "textref": "endnote.xhtml#note2"
            }
          ]
        }
      ]
    },
    {
      "role": ["paragraph"],
      "text": "This is a paragraph without a footnote."
    }
  ]
}
```

### Read-aloud output

**User preference set to read notes at the end of the text**

* This text has a footnote in the same resource and an endnote.
* Start of the footnote. Text of the footnote. End of the footnote.
* Start of the endnote. Text of the endnote. End of the endnote.
* This is a paragraph without a footnote.

**User preference set to read notes in the middle of the text**

* This text has a footnote (Start of the footnote. Text of the footnote. End of the footnote.) in the same resource and an endnote (Start of the endnote. Text of the endnote. End of the endnote.).
* This is a paragraph without a footnote.

**User preference set to skip notes**

* This text has a footnote in the same resource and an endnote.
* This is a paragraph without a footnote.