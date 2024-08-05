# Readium Guided Navigation

Guided navigation offers an alternative reading experience through a sequence of media fragments. In some cases, multiple media fragments are meant to be presented in parallel.

This document defines a syntax for Guided Navigation Documents, serialized in JSON and meant to be referenced in a [Readium Web Publication Manifest](https://readium.org/webpub-manifest).

**Editors:**

* Hadrien Gardeur

**Participate:**

* [GitHub](https://github.com/readium/guided-navigation/)
* [Discussions](https://github.com/readium/guided-navigation/discussions/)

## Use cases

* Synchronizing text with pre-recorded audio, for example in order to support [Media Overlays in EPUB](https://www.w3.org/TR/epub/#sec-media-overlays) or to distribute accessible audiobooks.
* Extracting a Guided Navigation Document from an HTML/XHTML resource for a read aloud feature or a reader mode.
* Panel by panel navigation in comics/manga to facilitate reading on smaller screens.
* Providing a textual transcript and/or image descriptions for highly illustrated publications such as [Divina](https://readium.org/webpub-manifest/profiles/divina).


## 1. Guided Navigation Documents

Guided Navigation Documents <strong class="rfc">must</strong> be identified using the following media type: `application/guided-navigation+json`.

### 1.1. Top-level properties

| Name | Description | Format | Required? |
| ---- | ----------- | ------ | ---------- |
| `links` | References to other resources that are related to the current Guided Navigation Document. | An array of [Link Objects](https://readium.org/webpub-manifest/#24-the-link-object) | No |
| `guided` | A sequence of resources and/or media fragments into these resources, meant to be presented sequentially to the user. | An array of [Guided Navigation Objects](#12-guided-navigation-object) | Yes |

*Example 1: A link to a subsequent Guided Navigation Document*

```json
{
  "links": [
    {
      "rel": "next",
      "href": "guided-chapter2.json"
    }
  ],
  "guided": [
    {
      "textref": "chapter1.html",
      "role": ["chapter"]
    }
  ]
}
```

### 1.2. Guided Navigation Object


| Name | Description | Format |
| ---- | ----------- | ------ |
| `audioref` | References an audio resource or a fragment of it. | URI |
| `children` | Items that are children of the containing Guided Navigation Object. | Guided Navigation Objects |
| `imgref` | References an image or a fragment of it. | URI |
| `role`     | Convey the structural semantics of a publication | Array of [roles](#4-roles) |
| `text`  | Textual equivalent of the resources or fragment of the resources referenced by the current Guided Navigation Object. | String |
| `textref`  | References a textual resource or a fragment of it. | URI |

Each Guided Navigation Object <strong class="rfc">must</strong> either contain:

- a `children` object containg at least one Guided Navigation Object
- or one of the following elements: `audioref`, `imgref` or `textref`

## 2. Relationship to the Readium Web Publication Manifest

Guided Navigation Documents are primarily designed to be referenced from a [Readium Web Publication Manifest](https://readium.org/webpub-manifest), in order to enable use cases that such manifests cannot handle on their own.

For example, a publication using the [EPUB profile](https://readium.org/webpub-manifest/profiles/epub) could handle [Media Overlays](https://www.w3.org/TR/epub-33/#sec-media-overlays) by parsing [SMIL files](https://www.w3.org/TR/SMIL3/) contained in the EPUB package and exposing them as Guided Navigation Documents.

### 2.1. Metadata requirements

If a publication references any Guided Navigation Document using `audioref`:

- it <strong class="rfc">must</strong> include a [`duration`](https://readium.org/webpub-manifest/contexts/default/#duration-and-number-of-pages) with the total duration for all audio resources referenced
- and it <strong class="rfc">should</strong> include one or more `narrator` 

### 2.2. Discovering a Guided Navigation Document

Users <strong class="rfc">must</strong> be free to move in and out of a Guided Navigation experience at will. 

To do so, publications <strong class="rfc">should</strong> provide references to Guided Navigation Documents both at a publication-level and as an alternate to resources referenced in the `readingOrder`.

#### 2.2.1 Publication-level

All valid [Readium Web Publication Manifests](https://readium.org/webpub-manifest) <strong class="rfc">must</strong> contain a `links` property with at least one reference to its canonical location.

In order to indicate the presence of a Guided Navigation Document, `links` <strong class="rfc">may</strong> also include an additional Link Object where:

- `type` uses the dedicated value for Guided Navigation Documents: `application/guided-navigation+json`
- and the `rel` value simply indicates that the Guided Navigation Document is `related` to the publication

Subsequent Guided Navigation Documents can be linked from this initial resource using the `links` property, in order to navigate throughout the publication.

*Example 2: A publication links to a Guided Navigation Document*

```json
"links": [
  {
    "rel": "self",
    "href": "https://example.com/manifest.json",
    "type": "application/guided-navigation+json"
  }
  {
    "rel": "related",
    "href": "https://example.com/guided.json",
    "type": "application/guided-navigation+json"
  }
]
```

#### 2.2.2 Link-level

In addition to a publication-level link, all resources in the `readingOrder` <strong class="rfc">should</strong> also point to the Guided Navigation Document that references them using the `alternate` property.

```json
"readingOrder": [
  {
    "href": "page1.jpg",
    "type": "image/jpeg",
    "alternate": [
      {
        "href": "guided.json",
        "type": "application/guided-navigation+json"
      }
    ]
  }
]
```

## 3. Fragments

> The following media fragments have been identified as potential candidates for fragments in `audioref`, `imgref` and `textref`:
> 
>- Audio: <https://www.w3.org/TR/media-frags/#naming-time>
>- Images:
>  - Rectangular regions: <https://www.w3.org/TR/media-frags/#naming-space>
>  - Polygonal regions: <https://idpf.org/epub/renditions/region-nav/#sec-3.5.1>
>- Text:
>  - Fragment ID: `#identifier` 
>  - Text fragments: <https://wicg.github.io/scroll-to-text-fragment/>
> 
> There are a number of open issues that relate to media fragments:
> 
> - Support for non-rectangular regions: <https://github.com/readium/guided-navigation/discussions/5>
> - Support for CSS selectors and positions in `textref`: <https://github.com/readium/guided-navigation/discussions/6>

## 4. Roles

> There are a number of open issues that relate to roles:
> 
> - Cherrypicking roles from EPUB 3 Structural Semantics Vocabulary 1.1: <https://github.com/readium/guided-navigation/discussions/2>
> - Identifying skippable and escapable roles: <https://github.com/readium/guided-navigation/discussions/3>
> - Defining a list of roles for Divina: <https://github.com/readium/guided-navigation/discussions/1>


Each Guided Navigation Object <strong class="rfc">should</strong> document one or more role using the `role` property.

Roles are inherited from multiple specifications such as [HTML](https://html.spec.whatwg.org/), [ARIA](https://www.w3.org/TR/wai-aria-1.1/), [DPUB ARIA](https://www.w3.org/TR/dpub-aria-1.1/) and [EPUB 3 Semantics Vocabulary](https://www.w3.org/TR/epub-ssv-11/), in order to convey the structural semantics of a publication.

The full list of supported roles is available at: <https://readium.org/guided-navigation/roles>

Roles can be used by reading applications to implement skippability (based on user preferences, some items could be skipped) and escapability (allowing users to escape from the current context, for example escaping from the content of a table to go back to the main text).

## Appendix A - JSON Schema

The following JSON Schemas are available under version control: 

- Guided Navigation Document: <https://github.com/readium/guided-navigation/blob/master/schema/document.schema.json>
- Guided Navigation Object: <https://github.com/readium/guided-navigation/blob/master/schema/object.schema.json>

For the purpose of validating a Readium Guided Navigation Document, use the following JSON Schema resource: 

- <https://readium.org/guided-navigation/schema/document.schema.json>


## Appendix B - Examples

*Example 1: Synchronizing text with pre-recorded audio*

```json
{
  "guided": [
    {
      "textref": "chapter1.html#start",
      "role": ["chapter"],
      "children": [
        {
          "textref": "chapter1.html#par1", 
          "audioref": "chapter1.mp3#t=0,20"
        },
        {
          "textref": "chapter1.html#par2", 
          "audioref": "chapter1.mp3#t=20,28"
        }
      ]
    }
  ]
}
```

*Example 2: Accessible audiobook using text references*

```json
{
  "guided": [
    {
      "audioref": "chapter1.mp3#t=0,20",
      "textref": "chapter1.html#par1"
    },
    { 
      "audioref": "chapter1.mp3#t=20,28",
      "textref": "chapter1.html#par2"
    }
  ]
}
```

*Example 3: Accessible audiobook using embedded text*

```json
{
  "guided": [
    {
      "audioref": "chapter1.mp3#t=0,7",
      "text": "This is the first sentence in this audiobook."
    },
    { 
      "audioref": "chapter1.mp3#t=7,16",
      "text": "Which is followed by a second, slightly longer sentence."
    }
  ]
}
```

*Example 4: Panel by panel navigation in a manga*

```json
{
  "guided": [
    {
      "role": ["panel"],
      "imgref": "page10.jpg#xywh=percent:10,10,60,40"
    },
    {
      "role": ["panel"],
      "imgref": "page10.jpg#xywh=percent:70,50,30,50"
    }
  ]
}
```

*Example 5: Accessible comic with a description and textual equivalent of a bubble*

```json
{
  "guided": [
    {
      "role": ["page"],
      "imgref": "page10.jpg",
      "children": [
        {
          "role": ["panel"],
          "imgref": "page10.jpg#xywh=percent:10,10,60,40",
          "description": "Emily marche seule, elle semble perdue dans ses pensées. Derrière elle, deux policiers la suivent du regard et échangent à son propos.",
          "character": ["Emily", "Premier policier", "Deuxième policier"],
          "children": [
            {
              "imgref": "page10.jpg#xywh=percent:10,10,30,20",
              "character": ["Emily"],
              "children": [
                {
                  "role": ["thoughtBubble"],
                  "text": "Quel étrange sentiment."
                },
                {
                  "role": ["thoughtBubble"],
                  "text": "J'accède enfin à ce poste tant convoité, et pourtant…"
                }
              ]
            },
            {
              "imgref": "page10.jpg#xywh=percent:40,10,20,20",
              "character": ["Premier policier", "Deuxième policier"],
              "children": [
                {
                  "role": ["speechBubble"],
                  "text": "Hé, c'est pas elle, la fille qui a arrêté le type de l'autre soir, celui qu'on a envoyé à Wormwood ?",
                  "character": ["Premier policier"]
                },
                {
                  "role": ["speechBubble"],
                  "text": "Ouais ! Il paraît qu'Hawkins y serait passé si elle n'avait pas été là.",
                  "character": ["Deuxième policier"]
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## Appendix C - Potential additions

### Potential object representation for `text`

| Name | Description | Format |
| ---- | ----------- | ------ |
| `language` | Main language for the text, which can be locally overriden in the SSML representation. | BCP-47 language tag |
| `plain` | Contains a plain text representation of the text. | String |
| `ssml` | Contains an SSML representation of the text. | SSML |


*Example: Text representation with both plain text and SSML*

```json
{
    "imgref": "page10.jpg#xywh=percent:10,10,20,20",
    "text": {
      "plain": "The SSML standard is defined by the W3C.",
      "language": "en",
      "ssml": "<speak>The <say-as interpret-as=\"characters\">SSML</say-as>standard <break time=\"1s\"/>is defined by the<sub alias=\"World Wide Web Consortium\">W3C</sub>.</speak>"
    }
}
```

### Potential format for `description`

| Name | Description | Format |
| ---- | ----------- | ------ |
| `description` | Text, audio or image description for the current Guided Navigation Object. | Guided Navigation Object without `children` |

*Example: Audio and text description for an image*

```json
{
  "role": ["panel"],
  "imgref": "page10.jpg#xywh=percent:10,10,60,40",
  "description": {
    "text": "A cowboy is looking at the city as the sun sets into the horizon.",
    "audioref": "description.mp3t=0,5"
  },
  "children": [
    {
      "role": ["speechBubble"],
      "imgref": "page10.jpg#xywh=percent:10,10,20,20",
      "text": "This is a dialogue in a speech bubble."
    }
  ]
}
```

### Potential format for `multipleImages`

> References:
>  
> * [EPUB Region-Based Navigation 1.0](https://idpf.org/epub/renditions/region-nav/#sec-3.5.2)
> * [Example](https://idpf.org/epub/renditions/region-nav/#app-a.2)

| Name | Description | Format |
| ---- | ----------- | ------ |
| `multipleImages` | Two or more references to images or regions of different images. | Array of URI |

*Example: Two page spread that contains a panel across the entire spread*

```json
{
  "role": ["spread"],
  "multipleImages": [
    "page2.jpg", 
    "page3.jpg"
  ]
  "children": [
    {
      "role": ["panel"]
      "multipleImages": [
        "page2.jpg#xywh=percent:0,0,100,20",
        "page3.jpg#xywh=percent:0,0,100,20"
      ]
    }
  ]
}
```

### Potential format for `character`


| Name | Description | Format |
| ---- | ----------- | ------ |
| `character` | Identifies one or more character present in a given Guided Navigation object. | Array of strings |

*Example: Characters present in the panel of a comicbook*

```json
{
  "role": ["panel"],
  "imgref": "page10.jpg#xywh=percent:10,10,60,40",
  "description": "Emily marche seule, elle semble perdue dans ses pensées. Derrière elle, deux policiers la suivent du regard et échangent à son propos.",
  "character": ["Emily", "Premier policier", "Deuxième policier"]
}  
```