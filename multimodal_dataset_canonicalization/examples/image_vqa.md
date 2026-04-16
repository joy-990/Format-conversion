# Image VQA Example

## Scenario

A dataset sample contains:
- one image
- one natural-language question
- one answer or answer list

## Canonical sample example

```json
{
  "id": "21",
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "image_url",
          "image_url": {
            "dataset_type": "train",
            "storage_type": "ceph",
            "url": "coco-text/COCO_train2014_000000214792.jpg"
          }
        },
        {
          "type": "text",
          "text": "Who is holding the mini jet?"
        }
      ]
    }
  ],
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": [
          {
            "type": "text",
            "text": "U.S. AIR FORCE"
          }
        ]
      }
    }
  ]
}
```
