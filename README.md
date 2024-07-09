# Guided Navigation

Guided navigation offers an alternative reading experience through a sequence of media fragments, meant to be presented in parallel.

This document defines a syntax for Guided Navigation Documents, serialized in JSON and meant to be references in a [Readium Web Publication Manifest](https://readium.org/webpub-manifest).

## Use cases

* Synchronizing text with pre-recorded audio, for example in order to support [Media Overlays in EPUB](https://www.w3.org/TR/epub/#sec-media-overlays) or to distribute accessible audiobooks.
* Panel by panel navigation in comics/manga to facilitate reading this type of content on smaller screens.
* Providing a textual transcript and/or image descriptions in a [Divina publication](https://readium.org/webpub-manifest/profiles/divina).

## Syntax

The new syntax is based around a single JSON object called a Guided Navigation Object:

| Name | Description | Format |
| ---- | ----------- | ------ |
| `audioref` | Points to a media fragment in an audio resource. | URI |
| `children` | Array of Guided Navigation Objects. | Guided Navigation Objects |
| `imgref` | Points to a media fragment in an image resource. | URI |
| `role`     | Array of roles relevant for the current Sync Media Object. | Array of roles |
| `text`  | Text equivalent for the current Guided Navigation Object. | String |
| `textref`  | Points to a media fragment in an HTML/XHTML resource. | URI |

Each Guided Navigation Object <strong class="rfc">must</strong> contain:

- a `children` object containg at least one Guided Navigation Object
- or one of the following elements: `audioref`, `imgref`, `text` or `textref`

## Roles

> As a starting point, we'll use the [EPUB Structural Semantics Vocabulary](https://www.w3.org/TR/epub-ssv-11/) along with SMIL as starting points for our list of roles. 
> 
> This will most likely end up with a mix of general and specialized vocabularies.

## Fragments

> The following media fragments have been identified as potential candidates for fragments in `audioref`, `imgref` and `textref`:
> 
>- Audio: <https://www.w3.org/TR/media-frags/#naming-time>
>- Images:
>  - Rectangular regions: <https://www.w3.org/TR/media-frags/#naming-space>
>  - Polygonal regions: <https://idpf.org/epub/renditions/region-nav/#sec-3.5.1>
>- Text:
>  - Fragment ID: `#identifier` 
>  - Text fragments: <https://wicg.github.io/scroll-to-text-fragment/>

## Linking to a Guided Navigation Document

### Publication-level

```json
"links": [
  {
    "rel": "alternate",
    "href": "guided.json",
    "type": "application/guided-navigation+json"
  }
]
```

### Link-level

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

## Publication metadata

A publication <strong class="rfc">must</strong> include a [`duration`](https://readium.org/webpub-manifest/contexts/default/#duration-and-number-of-pages) if it provides guided navigation using audio.

## Examples

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

## TODO

- Spreads: [Region-based navigation](https://idpf.org/epub/renditions/region-nav/#sec-3.5.2) and [example](https://idpf.org/epub/renditions/region-nav/#app-a.2)
- Descriptions

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