# Documentation related to PART A

## Platforms in scope

- Make.com
- Airtable
- JSON2Video
- ChatGPT
- Pexels

## General Steps regarding Airtable

- Create an account at: https://www.airtable.com/
- Create a workspace (called whatever you like)
- Create a base (called whatever you like)
- Create a table with the following columns:
    - Order (Number)
    - Line (Single line text)
    - Photo (Attachment)
- Write a personalized #short form video script (~3 lines)
- Find images to accompany each line of the script from:
    - ChatGPT
    - Google Images
    - Pexels
    - etc.

## General Steps regarding Make.com

- Add an `Airtable - Search Record` node
- Connect Airtable with Make.com through the `Builder Hub`
    - Create a `Personal Access Token` with the following "scopes":
        - data.records:read
        - data.records:write
        - schema.bases:read
    - Plus give the `Personal Access Token` the ability to interact the your base too
- Add a `Tools - Compose a string` node
```json
{
    "comment": "Scene {{1.Order}}",
    "elements": [
        {
            "type": "image",
            "src": "{{1.Photo[].url}}",
            "zoom": -1,
            "position": "center-center",
            "volume": 0
        },
        {
            "type": "voice",
            "text": "{{1.Line}}"
        }  
    ]
}
```
- Add a `JSON - Parse JSON` node
- Add a `Flow Control - Array aggregator` node
- Add a `JSON - Transform to JSON` node
- Add a `JSON2Video - Create a movie from JSON` node
```json
{
  "comment": "Slideshow",
  "resolution": "full-hd",
  "quality": "high",
  "scenes": 5.json,
  "elements": [
    {
      "type": "subtitles",
      "settings": {
        "font-family": "Boldonse-Regular",
                        "font-url": "https://www.dropbox.com/scl/fi/9x84netesf7acx9d6qml4/Boldonse-Regular.ttf?rlkey=bps6ymww8oe67vcg1f4bl1uqq&st=fp5e1jic&raw=1",
                        "box-color": "#000000",
                        "font-size": 160,
                        "word-color": "#FFFFFF",
                        "position": "center-center",
                        "style": "boxed-word",
                        "outline-color": "#000000",
                        "outline-width": 2,
                        "shadow-color": "#000000",
                        "shadow-offset": 4,
                        "max-words-per-line": 4
      }
    }
  ]
}
```

## General steps regarding JSON2Video

- Copy an API key from your JSON2Video dashboard into the JSON2Video node to form a connection
