This page documents the Cobalt Runtime results object model. The goal of the design is to create an object model that is intuitive, is programming language agnostic, yet extensible to future iterations.

The object model is encoded in JSON, making the object agnostic and easily accessible across modern languages. Clients of Cobalt Runtime may transmit this model across machines as-is, or wrap this model in additional JSON structures.

**Top Level Structure**: the top level structure of the JSON contains these fields

```
{
    "version": "1", 
    "features" : {},
    "cobalt_object": "stt_result", 
    "status": "final",
    "nbest": []
    "confusion_net":[]
}
```
The value "stt_result" denotes that this is a speech-to-text result.

The **Nbest structure** is a list, with an item in the list taking the following form:
```
       {
           "confidence": 1000,
           "features": {},
           "hypothesis": [
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {}, 
                    "value": "let's"
                }, 
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {}, 
                    "value": "recognize"
                }, 
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {}, 
                    "value": "speech"
                }
            ]
       },
```
The key "hypothesis" holds the textual contents of the result in the form of a list, it representing all the tokens, information about those tokens, that made up this result; the "features" key is an optional field, which may hold optional information relating to the entire hypothesis; the "confidence" key holds the confidence score for this hypothesis, it is an integer of range 0-1000.

**Hypothesis Features**
The features field associated with each item in the Nbest list may optionally include additional information such as perplexity.
Here is a list of feature fields:

| Feature       | Value       | Explanation |
| ------------- |-------------|-------------|
| perplexity    | float       | Perplexity calculated given the hypothesis and grammar FST provided in the config file |

Each element of the "hypothesis" list holds the keys "confidence", an integer score of range 0-1000; "value", the text of the result, "type", the type of the element, and "features", an optional field to hold additional information.

Here is a full example of an "stt_result" structure: [Full result example](stt_result_structure.md)

**Empty Results**: When a result is empty, no json is output. (Use the session_end logging callback if you need to determine whether processing has finished.)

**Semantic Tagging** In addition to tokens, models have the ability to tag a group of tokens with some action or intent:

<goto> navigate to </goto> <street_address> five four four one </street_address>

As the example shows, the tags <goto> and </goto> surround the tokens "navigate to". We are following XML conventions here and using "/" to denote the end of a tag.

We will work to define a limited set of actionable tags with the end-customer(s).

Here is the JSON representation of tagging:

```
           "hypothesis": [
                {
                    "type": "tag", 
                    "value": "<goto>"
                }, 
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {}, 
                    "value": "navigate"
                }, 
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {}, 
                    "value": "to"
                }
                {
                    "type": "tag", 
                    "value": "</goto>"
                }, 
            ],
```
Notice that tags do not have the confidence, or features keys.

**Token Features**
The features field associated with each token may optionally include additional information such as timestamps.
Here is a list of feature fields:

| Feature       | Value       | Explanation |
| ------------- |-------------|-------------|
| begin_ms      | integer | Start time (in milliseconds) of a token, relative to the beginning of a recognizer instance
| duration_ms   | integer | Duration (in milliseconds) of a token |

Here is an example for timestamps:
```
           "hypothesis": [
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {"begin_ms":0,"duration_ms":290}, 
                    "value": "let's"
                }, 
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {"begin_ms":290,"duration_ms":840}, 
                    "value": "recognize"
                }, 
                {
                    "confidence": 1000, 
                    "type": "token", 
                    "features": {"begin_ms":1130,"duration_ms":590}, 
                    "value": "speech"
                }
            ],
```

**Confusion Network**
The json output that represents a confusion network consists of a sequence of links, where each link groups a set of alternate arcs or tokens. Each link is associated with beginning and ending timestamps, which are overall average begin/end times for the set of tokens in a link.

Link:
```
{
"begin_ms":0,
"duration_ms":0,
"arcs":[]  //list of token-structs
}
```
Arc (token struct):
```
{
"confidence": 1000
"value": "let's"
}
```

Again, see a full example of an "stt_result" structure, including a confusion network, at: [Full result example](stt_result_structure.md)

