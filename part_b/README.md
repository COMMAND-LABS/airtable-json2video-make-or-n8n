# Documentation related to PART B

## Platforms in scope

- n8n
- Airtable
- JSON2Video

## General Steps regarding n8n

- Add a `Trigger Manually` node
- Add an `Airtable - Search records` node
    - Create a "new credential" to authenticate n8n with Airtable
- Add an `Edit Fields` node
    - Switch node into JSON mode
    - switch JSON field into "expression" mode
```json
{
    "comment": "Scene {{ $json.Order }}",
    "elements": [
        {
            "type": "image",
            "src": "{{ $json.Photo[0].url }}",
            "zoom": -1,
            "position": "center-center",
            "volume": 0
        },
        {
            "type": "voice",
            "text": "{{ $json.Line }}"
        }
    ]
}
```
- Add an `Aggregate` node
    - Put output in Field: `scenes`
- Add an `HTTP Request` node
    - import cURL
    - cURL found here: `https://json2video.com/docs/v2/api-reference/api-endpoints/movies#post-movies---create-a-new-movie`
```sh
curl --location --request POST 'https://api.json2video.com/v2/movies' \
--header 'x-api-key: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "scenes": [
        {
            "elements": [
                {
                    "type": "video",
                    "src": "https://example.com/path/to/my/video.mp4"
                }
            ]
        }
    ]
}'
```
- Edit body of `HTTP Request` node sent to the JSON2Video API
```json
{
    "comment": "Movie",
    "resolution": "full-hd",
    "quality": "high",
    "scenes": JSON.stringify($json.scenes),
    "elements": [
        {
            "type": "subtitles",
            "settings": {
                "font-family": "Boldonse-Regular",
                "font-url": "https://www.dropbox.com/scl/fi/9x84netesf7acx9d6qml4/Boldonse-Regular.ttf?rlkey=bps6ymww8oe67vcg1f4bl1uqq&st=fp5e1jic&raw=1",
                "box-color": "#8a112d",
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
