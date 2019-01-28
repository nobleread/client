```
{
  "message": "returning results",
  "results": [
    {
      "status": "partial",
      "version": "1",
      "cobalt_object": "stt_result",
      "features": null,
      "nbest": [
        {
          "confidence": 1000,
          "features": null,
          "hypothesis": [
            {
              "confidence": 1000,
              "type": "token",
              "features": null,
              "value": "Hello"
            }
          ]
        }
      ]
    },
    {
      "status": "final",
      "features": null,
      "version": "1",
      "cobalt_object": "stt_result",
      "nbest": [
        {
          "confidence": 1000,
          "features": {
              "perplexity": 30.0
          },
          "hypothesis": [
            {
              "confidence": 1000,
              "type": "token",
              "features": {
                "duration_ms": 330,
                "begin_ms": 660
              },
              "value": "Hello"
            },
            {
              "confidence": 1000,
              "type": "token",
              "features": {
                "duration_ms": 600,
                "begin_ms": 990
              },
              "value": "World"
            }
          ]
        }
      ],
      "confusion_network": [
        {
          "duration_ms": 654,
          "begin_ms": 0,
          "object_name": "link",
          "arcs": [
            {
              "confidence": 1000,
              "value": "<eps>"
            }
          ]
        },
        {
          "duration_ms": 317,
          "begin_ms": 654,
          "object_name": "link",
          "arcs": [
            {
              "confidence": 450,
              "value": "HELLO"
            },
            {
              "confidence": 203,
              "value": "OH"
            },
            {
              "confidence": 170,
              "value": "A"
            },
            {
              "confidence": 106,
              "value": "THE"
            },
            {
              "confidence": 34,
              "value": "THOUGH"
            },
            {
              "confidence": 15,
              "value": "LOW"
            },
            {
              "confidence": 12,
              "value": "FOR"
            },
            {
              "confidence": 6,
              "value": "LITTLE"
            },
            {
              "confidence": 3,
              "value": "O"
            }
          ]
        },
        {
          "duration_ms": 5,
          "begin_ms": 971,
          "object_name": "link",
          "arcs": [
            {
              "confidence": 973,
              "value": "<eps>"
            },
            {
              "confidence": 12,
              "value": "THE"
            },
            {
              "confidence": 8,
              "value": "LOW"
            },
            {
              "confidence": 6,
              "value": "LITTLE"
            }
          ]
        },
        {
          "duration_ms": 613,
          "begin_ms": 977,
          "object_name": "link",
          "arcs": [
            {
              "confidence": 1000,
              "value": "WORLD"
            }
          ]
        },
        {
          "duration_ms": 500,
          "begin_ms": 1590,
          "object_name": "link",
          "arcs": [
            {
              "confidence": 1000,
              "value": "<eps>"
            }
          ]
        }
      ]
    }
  ]
}
```