# Guided navigation

This document introduces a new JSON based syntax to provide a guided navigation experience in a Readium Web Publication Manifest.

## Model

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

- <https://www.w3.org/TR/epub-ssv-11/>

## Fragments

- Audio: <https://www.w3.org/TR/media-frags/#naming-time>
- Images:
  - Rectangular regions: <https://www.w3.org/TR/media-frags/#naming-space>
  - Polygonal regions: <https://idpf.org/epub/renditions/region-nav/#sec-3.5.1>
- Text:
  - Fragment ID: `#identifier` 
  - Text fragments: <https://wicg.github.io/scroll-to-text-fragment/>

## Listing Guided Navigation Documents

### Publication-level

```json
"links": [
  {
    "rel": "alternate,
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

*Example 1: Guided navigation in a comicbook*

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

*Example 2: Guided navigation between text and audio*

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

*Example 3: Text equivalent of a speech bubble in a comic book*

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

- [ ] Synthetic spread: [spec](https://idpf.org/epub/renditions/region-nav/#sec-3.5.2) and [example](https://idpf.org/epub/renditions/region-nav/#app-a.2)
- [ ] Descriptions ?

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