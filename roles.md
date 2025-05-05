# Roles

## Inherited from HTML and/or ARIA

| Role | EPUB type equivalent | ARIA role | HTML element | Definition |
| ---- | -------------------- | --------- | ------------ | ---------- |
| `article` | | | `<article>` | Represents a self-contained composition in a document, page, application, or site, which is intended to be independently distributable or reusable |
| `aside` | [`aside`](https://www.w3.org/TR/epub-ssv-11/#sec-asides) | | `<aside>` | Secondary or supplementary content. |
| `audio` | | | `<audio>` | Embedded sound content in a document. |
| `blockquote` | | | `<blockquote>` | Represents a section that is quoted from another source. |
| `caption` | | | `<caption>` or `<figcaption>` | A caption for an image or a table. |
| `cell` | [`table-cell`](https://www.w3.org/TR/epub-ssv-11/#sec-tables) |  `cell` | `<td>` | A single cell of tabular data or content. |
| `columnheader` |  |  `columnheader` | `<th>` with scope `col` | The header cell for a column, establishing a relationship between it and the other cells in the same column. |
| `complementary` | | `complementary` | | A supporting section of the document, designed to be complementary to the main content at a similar level in the DOM hierarchy, but remains meaningful when separated from the main content. |
| `definition` | [`glossdef`](https://w3c.github.io/epub-specs/epub33/ssv/#h_glossaries) | `definition` | `<dd>` | A definition of a term or concept. |
| `details` | | | `<details>` | A disclosure widget that can be expanded. |
| `figure` | [`figure`](https://www.w3.org/TR/epub-ssv-11/#sec-figures) | `figure` | `<figure>` | An illustration, diagram, photo, code listing or similar, referenced from the text of a work, and typically annotated with a title, caption and/or credits. |
| `header` | | | `<header>` | Represents introductory content, typically a group of introductory or navigational aids. |
| `heading` | | `heading` | `<h1>` through `<h6>` | A heading for a section of the page. |
| `image` | | `img` | `<img>` | Represents an image. |
| `list` | [`list`](https://www.w3.org/TR/epub-ssv-11/#sec-lists) | `list` | `<ul>` or `<ol>` | A structure that contains an enumeration of related content items. |
| `listItem` | [`list-item`](https://www.w3.org/TR/epub-ssv-11/#sec-lists)  | `listitem`| `<li>` |  A single item in an enumeration. |
| `main` | | `main` | | Content that is directly related to or expands upon the central topic of the document. |
| `math` | | `math` | `<math>` | Content that represents a mathematical expression. |
| `navigation` | | `navigation` | `<nav>` | Represents a section of a page that links to other pages or to parts within the page: a section with navigation links. |
| `paragraph` | | | `<p>` | Represents a paragraph. |
| `preformatted` | | | `<pre>` | Represents preformatted text which is to be presented exactly as written. |
| `presentation` | | `presentation` or `none` | | Represents an element being used only for presentation and therefore that does not have any accessibility semantics. |
| `region` | | `region` | | Represents content that is relevant to a specific, author-specified purpose and sufficiently important that users will likely want to be able to navigate to the section easily and to have it listed in a summary of the page. |
| `row` | [`table-row`](https://www.w3.org/TR/epub-ssv-11/#sec-tables) | `row` | `<tr>` | A row of data or content in a tabular structure. |
| `rowheader` |  | `rowheader` | `<th>` scoped to `row` | The header cell for a row, establishing a relationship between it and the other cells in the same row. |
| `section` | | | `<section>` | Represents a generic standalone section of a document, which doesn't have a more specific semantic element to represent it. |
| `separator` | | `separator` | `<hr>` | Indicates the element is a divider that separates and distinguishes sections of content or groups of menuitems. |
| `summary` | | | `<summary>` | A summary of an element contained in details. |
| `table` | [`table`](https://www.w3.org/TR/epub-ssv-11/#sec-tables) | `table` | `<table>` | A structure containing data or content laid out in tabular form. |
| `term` | [`glossterm`](https://w3c.github.io/epub-specs/epub33/ssv/#h_glossaries) | `term` | `<dfn>` or `<dt>` | A word or phrase with a corresponding definition.|
| `video` | | | `<video>` | Embedded videos, movies, or audio files with captions in a document.|

## Inherited from DPUB ARIA 1.0

| Role | ARIA equivalent | EPUB type equivalent | Definition |
| ---- | --------------- | -------------------- | ---------- |
| `abstract` | [`doc-abstract`](https://www.w3.org/TR/dpub-aria-1.0/#doc-abstract) | [`abstract`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | A short summary of the principle ideas, concepts and conclusions of the work, or of a section or excerpt within it. |
| `acknowledgments` | [`doc-acknowledgments`](https://www.w3.org/TR/dpub-aria-1.0/#doc-acknowledgments) | [`acknowledgments`](https://www.w3.org/TR/epub-ssv-11/#sec-preliminary) | A section or statement that acknowledges significant contributions by persons, organizations, governments and other entities to the realization of the work. |
| `afterword` | [`doc-afterword`](https://www.w3.org/TR/dpub-aria-1.0/#doc-afterword) | [`afterword`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | A closing statement from the author or a person of importance, typically providing insight into how the content came to be written, its significance, or related events that have transpired since its timeline. |
| `appendix` | [`doc-appendix`](https://www.w3.org/TR/dpub-aria-1.0/#doc-appendix) | [`appendix`](https://www.w3.org/TR/epub-ssv-11/#sec-document-references) | A section of supplemental information located after the primary content that informs the content but is not central to it. |
| `backlink` | [`doc-backlink`](https://www.w3.org/TR/dpub-aria-1.0/#doc-backlink) | [`backlink`](https://www.w3.org/TR/epub-ssv-11/#links) | A link that allows the user to return to a related location in the content (e.g., from a footnote to its reference or from a glossary definition to where a term is used). |
| `bibliography` | [`doc-bibliography`](https://www.w3.org/TR/dpub-aria-1.0/#doc-bibliography) | [`bibliography`](https://www.w3.org/TR/epub-ssv-11/#h_bibliographies) | A list of external references cited in the work, which may be to print or digital sources.|
| `biblioref` | [`doc-biblioref`](https://www.w3.org/TR/dpub-aria-1.0/#doc-biblioref) | [`biblioref`](https://www.w3.org/TR/epub-ssv-11/#links) | A reference to a bibliography entry.|
| `chapter` | [`doc-chapter`](https://www.w3.org/TR/dpub-aria-1.0/#doc-chapter) | [`chapter`](https://www.w3.org/TR/epub-ssv-11/#sec-divisions) | A major thematic section of content in a work. |
| `colophon` | [`doc-colophon`](https://www.w3.org/TR/dpub-aria-1.0/#doc-colophon) | [`colophon`](https://www.w3.org/TR/epub-ssv-11/#sec-document-references) | A short section of production notes particular to the edition (e.g., describing the typeface used), often located at the end of a work. |
| `conclusion` | [`doc-conclusion`](https://www.w3.org/TR/dpub-aria-1.0/#doc-conclusion) | [`conclusion`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | A concluding section or statement that summarizes the work or wraps up the narrative. |
| `cover` | [`doc-cover`](https://www.w3.org/TR/dpub-aria-1.0/#doc-cover) | [`cover`](https://www.w3.org/TR/epub-ssv-11/#sec-partitions) | An image that sets the mood or tone for the work and typically includes the title and author. |
| `credit` | [`doc-credit`](https://www.w3.org/TR/dpub-aria-1.0/#doc-credit) | [`credit`](https://www.w3.org/TR/epub-ssv-11/#document-text) | An acknowledgment of the source of integrated content from third-party sources, such as photos. Typically identifies the creator, copyright and any restrictions on reuse. |
| `credits` | [`doc-credits`](https://www.w3.org/TR/dpub-aria-1.0/#doc-credits) | [`credits`](https://www.w3.org/TR/epub-ssv-11/#sec-document-references) | A collection of credits. |
| `dedication` | [`doc-dedication`](https://www.w3.org/TR/dpub-aria-1.0/#doc-dedication) | [`dedication`](https://www.w3.org/TR/epub-ssv-11/#sec-preliminary) | An inscription at the front of the work, typically addressed in tribute to one or more persons close to the author. |
| `endnotes` | [`doc-endnotes`](https://www.w3.org/TR/dpub-aria-1.0/#doc-endnotes) | [`endnotes`](https://www.w3.org/TR/epub-ssv-11/#notes) | A collection of notes at the end of a work or a section within it. |
| `epigraph` | [`doc-epigraph`](https://www.w3.org/TR/dpub-aria-1.0/#doc-epigraph) | [`epigraph`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | A quotation set at the start of the work or a section that establishes the theme or sets the mood. |
| `epilogue` | [`doc-epilogue`](https://www.w3.org/TR/dpub-aria-1.0/#doc-epilogue) | [`epilogue`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | A concluding section of narrative that wraps up or comments on the actions and events of the work, typically from a future perspective. |
| `errata` | [`doc-errata`](https://www.w3.org/TR/dpub-aria-1.0/#doc-errata) | [`errata`](https://www.w3.org/TR/epub-ssv-11/#sec-preliminary) | A set of corrections discovered after initial publication of the work, sometimes referred to as corrigenda.|
| `example` | [`doc-example`](https://www.w3.org/TR/dpub-aria-1.0/#doc-example) | [`example`](https://www.w3.org/TR/epub-ssv-11/#dictionaries) | An illustration of the usage of a defined term or phrase.|
| `footnote` | [`doc-footnote`](https://www.w3.org/TR/dpub-aria-1.0/#doc-footnote) | [`footnote`](https://www.w3.org/TR/epub-ssv-11/#notes) | Ancillary information, such as a citation or commentary, that provides additional context to a referenced passage of text.|
| `glossary` | [`doc-glossary`](https://www.w3.org/TR/dpub-aria-1.0/#doc-glossary) | [`glossary`](https://www.w3.org/TR/epub-ssv-11/#h_glossaries) | A brief dictionary of new, uncommon, or specialized terms used in the content.|
| `glossref` | [`doc-glossref`](https://www.w3.org/TR/dpub-aria-1.0/#doc-glossref) | [`glossref`](https://www.w3.org/TR/epub-ssv-11/#links) | A reference to a glossary definition.|
| `index` | [`doc-index`](https://www.w3.org/TR/dpub-aria-1.0/#doc-index) | [`index`](https://www.w3.org/TR/epub-ssv-11/#h_indexes) | A navigational aid that provides a detailed list of links to key subjects, names and other important topics covered in the work.|
| `introduction` | [`doc-introduction`](https://www.w3.org/TR/dpub-aria-1.0/#doc-introduction) | [`introduction`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | A preliminary section that typically introduces the scope or nature of the work.|
| `noteref` | [`doc-noteref`](https://www.w3.org/TR/dpub-aria-1.0/#doc-noteref) | [`noteref`](https://www.w3.org/TR/epub-ssv-11/#links) | A reference to a footnote or endnote, typically appearing as a superscripted number or symbol in the main body of text.|
| `notice` | [`doc-notice`](https://www.w3.org/TR/dpub-aria-1.0/#doc-notice) | [`notice`](https://www.w3.org/TR/epub-ssv-11/#sec-complementary) | Notifies the user of consequences that might arise from an action or event. Examples include warnings, cautions and dangers.|
| `pagebreak` | [`doc-pagebreak`](https://www.w3.org/TR/dpub-aria-1.0/#doc-pagebreak) | [`pagebreak`](https://www.w3.org/TR/epub-ssv-11/#sec-pagination) | A separator denoting the position before which a break occurs between two contiguous pages in a statically paginated version of the content.|
| `pagelist` | [`doc-pagelist`](https://www.w3.org/TR/dpub-aria-1.0/#doc-pagelist) | [`page-list`](https://www.w3.org/TR/epub-ssv-11/#sec-pagination) | A navigational aid that provides a list of links to the pagebreaks in the content.|
| `part` | [`doc-part`](https://www.w3.org/TR/dpub-aria-1.0/#doc-part) | [`part`](https://www.w3.org/TR/epub-ssv-11/#sec-divisions) | A major structural division in a work that contains a set of related sections dealing with a particular subject, narrative arc or similar encapsulated theme. |
| `preface` | [`doc-preface`](https://www.w3.org/TR/dpub-aria-1.0/#doc-preface) | [`preface`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | An introductory section that precedes the work, typically written by the author of the work. |
| `prologue` | [`doc-prologue`](https://www.w3.org/TR/dpub-aria-1.0/#doc-prologue) | [`prologue`](https://www.w3.org/TR/epub-ssv-11/#sec-sections) | An introductory section that sets the background to a work, typically part of the narrative. |
| `pullquote` | [`doc-pullquote`](https://www.w3.org/TR/dpub-aria-1.0/#doc-pullquote) | [`pullquote`](https://www.w3.org/TR/epub-ssv-11/#sec-complementary) | A distinctively placed or highlighted quotation from the current content designed to draw attention to a topic or highlight a key point. |
| `qna` | [`doc-qna`](https://www.w3.org/TR/dpub-aria-1.0/#doc-qna) | [`qna`](https://www.w3.org/TR/epub-ssv-11/#h_testing) | A section of content structured as a series of questions and answers, such as an interview or list of frequently asked questions. |
| `subtitle` | [`doc-subtitle`](https://www.w3.org/TR/dpub-aria-1.0/#doc-subtitle) | [`subtitle`](https://www.w3.org/TR/epub-ssv-11/#sec-titles) | An explanatory or alternate title for the work, or a section or component within it. |
| `tip` | [`doc-tip`](https://www.w3.org/TR/dpub-aria-1.0/#doc-tip) | [`tip`](https://www.w3.org/TR/epub-ssv-11/#sec-complementary) | Helpful information that clarifies some aspect of the content or assists in its comprehension. |
| `toc` | [`doc-toc`](https://www.w3.org/TR/dpub-aria-1.0/#doc-toc) | [`toc`](https://www.w3.org/TR/epub-ssv-11/#sec-navigation) | A navigational aid that provides an ordered list of links to the major sectional headings in the content. A table of contents may cover an entire work, or only a smaller section of it. |

## Inherited from EPUB 3 Structural Semantics Vocabulary 1.1

| Role | EPUB type equivalent | Definition |
| ---- | -------------------- | ---------- |
| `landmarks` | [`landmarks`](https://w3c.github.io/epub-specs/epub33/ssv/#sec-navigation) | A short summary of the principle ideas, concepts and conclusions of the work, or of a section or excerpt within it. |
| `loa` | [`loa`](https://w3c.github.io/epub-specs/epub33/ssv/#sec-navigation) | A listing of audio clips included in the work. |
| `loi` | [`loi`](https://w3c.github.io/epub-specs/epub33/ssv/#sec-navigation) | A listing of illustrations included in the work. |
| `lot` | [`lot`](https://w3c.github.io/epub-specs/epub33/ssv/#sec-navigation) | A listing of tables included in the work. |
| `lov` | [`lov`](https://w3c.github.io/epub-specs/epub33/ssv/#sec-navigation) | A listing of video clips included in the work. |

## List of skippable roles

### Ancillary content

* `aside`
* `bibliography`
* `endnotes`
* `footnote`
* `noteref`
* `pullquote`

### Navigation

* `landmarks`
* `loa`
* `loi`
* `lot`
* `lov`
* `pagebreak`
* `toc`

## List of escapable roles

* `aside`
* `figure`
  * `caption` 
* `list`
  * `listItem` 	
* `table`
  * `columnheader`
  * `rowheader`
  * `row`
  * `cell`