# Guided Navigation

Guided navigation offers an alternative reading experience through a sequence of media fragments, meant to be presented in parallel.

This document defines a syntax for Guided Navigation Documents, serialized in JSON and meant to be references in a [Readium Web Publication Manifest](https://readium.org/webpub-manifest).

## Use cases

* Synchronizing text with pre-recorded audio, for example in order to support [Media Overlays in EPUB](https://www.w3.org/TR/epub/#sec-media-overlays) or to distribute accessible audiobooks.
* Panel by panel navigation in comics/manga to facilitate reading on smaller screens.
* Providing a textual transcript and/or image descriptions for high illustrated publications such as [Divina](https://readium.org/webpub-manifest/profiles/divina).


## 1. Guided Navigation Documents

Guided Navigation Documents <strong class="rfc">must</strong> be identified using the following media type: `application/guided-navigation+json`.

### 1.1. Top-level properties

| Name | Description | Format | Required? |
| ---- | ----------- | ------ | ---------- |
| `links` | References to other resources that are related to the current Guided Navigation Document. | An array of [Link Objects](https://readium.org/webpub-manifest/#24-the-link-object) | No |
| `guided` | A sequence of resources and/or media fragments into these resources, meant to be presented sequentially to the user. | An array of [Guided Navigation Objects](#12-guided-navigation-object) | Yes |

### 1.2. Guided Navigation Object


| Name | Description | Format |
| ---- | ----------- | ------ |
| `audioref` | Points to a media fragment in an audio resource. | URI |
| `children` | Array of Guided Navigation Objects. | Guided Navigation Objects |
| `imgref` | Points to a media fragment in an image resource. | URI |
| `role`     | Array of roles relevant for the current Guided Navigation Object. | Array of roles |
| `text`  | Textual equivalent of the resources or fragment of the resources referenced by the current Guided Navigation Object. | String |
| `textref`  | Points to a media fragment in an HTML/XHTML resource. | URI |

Each Guided Navigation Object <strong class="rfc">must</strong> either contain:

- a `children` object containg at least one Guided Navigation Object
- or one of the following elements: `audioref`, `imgref` or `textref`

## 2. Guided navigation in a publication

### 2.1. Metadata requirements

A publication <strong class="rfc">must</strong> include a [`duration`](https://readium.org/webpub-manifest/contexts/default/#duration-and-number-of-pages) if it references a Guided Navigation Document using audio.

### 2.2. Publication-level

```json
"links": [
  {
    "rel": "alternate",
    "href": "guided.json",
    "type": "application/guided-navigation+json"
  }
]
```

### 2.2. Link-level

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

## 4. Roles

> As a starting point, we'll use the [EPUB Structural Semantics Vocabulary](https://www.w3.org/TR/epub-ssv-11/) along with SMIL as starting points for our list of roles. 
> 
> This will most likely end up with a mix of general and specialized vocabularies.

## Appendix A - Examples

*Example 1: Synchronizing text with pre-recorded audio*

```json
{
  "textref": "chapter1.html",
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
```

*Example 2: Synchronizing an audiobook with text using references*

```json
{
  "textref": "chapter1.mp3",
  "role": ["chapter"],
  "children": [
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

*Example 3: Synchronizing an audiobook with text using embedded text*

```json
{
  "textref": "chapter1.mp3",
  "role": ["chapter"],
  "children": [
    {
      "audioref": "chapter1.mp3#t=0,7",
      "text": "This is the first sentence in this audiobook."
    },
    { 
      "audioref": "chapter1.mp3#t=8,16",
      "text": "Which is followed by a second, slightly longer sentence."
    }
  ]
}
```

*Example 4: Guided navigation in a manga*

```json
{
  "role": ["page"],
  "imgref": "page10.jpg",
  "children": [
    {
      "role": ["panel"]
      "imgref": "page10.jpg#xywh=percent:10,10,60,40"
    },
    {
      "role": ["panel"]
      "imgref": "page10.jpg#xywh=percent:70,50,30,50"
    }
  ]
}
```

*Example 5: Text equivalent of a speech bubble in a comic book*

```json
{
  "role": ["panel"],
  "imgref": "page10.jpg#xywh=percent:10,10,60,40",
  "children": [
    {
      "role": ["balloon"]
      "imgref": "page10.jpg#xywh=percent:10,10,20,20",
      "text": "This is a dialogue in a speech bubble."
    }
  ]
}
```

## Appendix B - JSON Schema

The following JSON Schemas are available under version control: 

- Guided Navigation Document: <https://github.com/readium/guided-navigation/blob/master/schema/document.schema.json>
- Guided Navigation Object: <https://github.com/readium/guided-navigation/blob/master/schema/object.schema.json>

For the purpose of validating a Readium Guided Navigation Document, use the following JSON Schema resource: 

- <https://readium.org/guided-navigation/schema/document.schema.json>

## Appendix C - TODO

- Spreads: [Region-based navigation](https://idpf.org/epub/renditions/region-nav/#sec-3.5.2) and [example](https://idpf.org/epub/renditions/region-nav/#app-a.2)
- Descriptions
- SSML support for `text`?

### Potential format for spreads

| Name | Description | Format |
| ---- | ----------- | ------ |
| `spread` | Two or more references to images or regions of different images. | URI |

*Example: Two pages spread that contains a panel across the entire spread*

```json
{
  "role": ["spread"],
  "spread": [
    "page2.jpg", 
    "page3.jpg"
  ]
  "children": [
    {
      "role": ["panel"]
      "spread": [
        "page2.jpg#xywh=percent:0,0,100,20",
        "page3.jpg#xywh=percent:0,0,100,20"
      ]
    }
  ]
}
```


### Potential format for descriptions

| Name | Description | Format |
| ---- | ----------- | ------ |
| `description` | Text, audio or image description for the current Guided Navigation Object. | Object |

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
      "role": ["balloon"]
      "imgref": "page10.jpg#xywh=percent:10,10,20,20",
      "text": "This is a dialogue in a speech bubble."
    }
  ]
}
```