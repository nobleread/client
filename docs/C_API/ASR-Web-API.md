# Entry points

There are two entry points: /api, and /recognize. The /api entry point is used to create and delete objects with state and query attributes. The /recognize entry point is used to send audio, send events to control recognition, and retrieve results. All requests to the API are POST, and all successful returns are JSON.

# Stateless Example

The following is a stateless example, where we start and finish a recognition in a single API call by sending a .wav file to the recognize entry point. Note that this entry point only accepts 16 KHz, 16 bit sampled, mono .wav files.

```
$ curl  --data-binary @<wav audio> -H "event-type: file"  -H"file-type: wav" <url>
{
   "message":"returning results",
   "results":[
      {
         "status":"final",
         "version":"1",
         "cobalt_object":"stt_result",
         "features":null,
         "nbest":[
            [
               {
                  "confidence":1000,
                  "type":"token",
                  "features":null,
                  "value":"hello"
               },
               {
                  "confidence":1000,
                  "type":"token",
                  "features":null,
                  "value":"world"
               }
            ]
         ]
      }
   ]
}
```

There are two possible outcomes of each file based API call are: no result is ready, or results are ready. The key "results" will exist if results ready. Because there may be multiple utterances per audio file, we return results as a list of stt_results, where each item of the list represents an utterance, and all items are in temporal order. See [[Results-Object-Model|Results Object Model]] for details on the stt_results format.

# /api entry point

The /api entry point contains API calls that create and delete recognizer objects containing state and query models for various state. The /api entry point accepts JSON formatted text data in the body. The JSON payload must contain the key "type". The following table lists the different types.

| type | required key(s) | description | return on success
|--- | --- | --- | ---
| get-model-attributes | model-id | get the attributes of a model | JSON object with key/value configuration pairs
| create-recognizer | model-id (uses default if no key provided) | creates a recognizer on the server | JSON object with 'recognizer-id'
| create-extended-recognizer | model-id (uses default if no key provided) | creates a long running recognizer on the server | JSON object with 'recognizer-id'
| delete-recognizer | recognizer-id | deletes a recognizer | JSON with message


### get-model-attributes

```
$ curl -X POST -H "Content-type: application/json" \
      -d '{"type": "get-model-attributes", "model-id": "test-model"}' \
      http://localhost:8888/api
{"sample-rate": 8000, "bits-per-sample": 16}
```

### create-recognizer
```
$ curl -X POST -H "Content-type: application/json" \
      -d '{"type": "create-recognizer", "model_id": "test-model"}' \
      http://localhost:8888/api
{"recognizer-id": "8d24725c-5b06-11e5-892c-a45e60f06c45"}
```

### delete-recognizer
```
$ curl -X POST -H "Content-type: application/json" \
      -d '{"type": "delete-recognizer", "recognizer-id": "8d24725c-5b06-11e5-892c-a45e60f06c45"}' \
      http://localhost:8888/api
{"message": "deleted recognizer 8d24725c-5b06-11e5-892c-a45e60f06c45"}
```

# /recognize entry point
The /recognize entry point expects information in the header, each request header must hold the key "event-type". The body (payload) should be binary audio data. The following table lists the different types of requests. Note that we've already shown the state-less API call above.

| event-type | required key(s) | description | body required | return on success
|--- | --- | --- | --- | ---
| file | file-type | stateless file based interaction | yes, binary wav file | JSON with list of stt_results in "results" key
| streaming-audio | recognizer-id | stream audio from the same speaker/device, a chunk at a time | yes, binary PCM | JSON with message
| clear-audio-queue | recognizer-id | returns when recognizer finishes processing audio | no | JSON with message
| end-session | recognizer-id | ends the recognition session, publishes result if available | no | JSON with message
| get-result | recognizer-id | gets recognition results for current recognizer | no | JSON with list of some kind of result in "results" key
| get-final-result | recognizer-id | combines clear-audio-session, end-session, get-result, returns all results, this call saves some round trip network latency, if the client is sure that it is done sending audio (client-side endpointing) | no | JSON with list of some kind of result in "results" key
Note that the results key returned by the get-result and get-final-result calls will contain [[Results-Object-Model|Results Object Model]] for stt_results.
# Streaming audio API

## Motivations, implementation, and workflow

Opening an entry point to allow streaming audio allows clients to push audio as soon as it is available. This allows the cloud-based engine to initialize computationally heavy machine learning algorithms only once, which decreases the latency of each subsequent API call. 

The following example uses Python's [requests](http://docs.python-requests.org/en/master/) package to illustrate making streaming API calls. Streaming audio requires session persistence, so please send server side cookies back if you plan to use the streaming audio API. Please contact Cobalt for our client library to see the full code.


## Example
```python

# we must make sure these calls hit the same backend server, so we use the sessions object to handle server cookies.
requests_obj = requests.Session()

# send request to /api entry point to create recognizer
recognizer_id = send_create_recognizer(api_url, requests_obj=requests_obj, verbose=verbose)

# send data a chunk at a time.
while begin_index < len(data):
    if begin_index + chunk - 1 <= len(data):
        end_index = begin_index + chunk
    else:
        end_index = len(data)
    send_stream_audio(requests_obj, recognize_url, recognizer_id, data[begin_index:end_index], verbose=verbose)
    begin_index += chunk

# send_stream_audio() is a non-blocking call, so we call clear_audio_queue to make sure it processed all the audio 
# an alternative is to make periodic calls with send_get_result() until we get an actionable result.
send_clear_audio_queue(requests_obj, recognize_url, recognizer_id, verbose=verbose)

# if the model does not 'end-point', this call forces an 'end-point'.
send_end_session(requests_obj, recognize_url, recognizer_id, verbose=verbose)

# get the result
results = send_get_result(requests_obj, recognize_url, recognizer_id)
send_delete_recognizer(api_url, recognizer_id, requests_obj=requests_obj, verbose=verbose)
```

## Long Audio Files

A special use case for the streaming mode is to decode very long audio files (a few minutes or longer). The key idea is to break up the long file into chunks and stream them into the server in synchronous mode. As each chunk is streamed to the server, the client should check for results, which will become available whenever end-points are reached. Note that the model must support end-pointing for the long audio file support to work. This special recognizer is created with the `create-extended-recognizer` type call against /api:

```Python
requests_obj = requests.Session()
recognizer_id = send_create_recognizer(api_url, recognizer_construct, is_extended=True, requests_obj=requests_obj)

results = []
for chunk in wav_file_iter(audio_filepath):
    send_stream_audio(requests_obj, recognize_url, recognizer_id, chunk)
    try:
        result = send_get_result(requests_obj, recognize_url, recognizer_id, expected_message='returning results')
        results.append(result)
    except Exception:
        pass

send_end_session(requests_obj, recognize_url, recognizer_id)
send_delete_recognizer(api_url, recognizer_id, requests_obj=requests_obj)
```