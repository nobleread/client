# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [cubic.proto](#cubic.proto)
    - [ConfusionNetworkArc](#cobaltspeech.cubic.ConfusionNetworkArc)
    - [ConfusionNetworkLink](#cobaltspeech.cubic.ConfusionNetworkLink)
    - [ListModelsRequest](#cobaltspeech.cubic.ListModelsRequest)
    - [ListModelsResponse](#cobaltspeech.cubic.ListModelsResponse)
    - [Model](#cobaltspeech.cubic.Model)
    - [ModelAttributes](#cobaltspeech.cubic.ModelAttributes)
    - [RecognitionAlternative](#cobaltspeech.cubic.RecognitionAlternative)
    - [RecognitionAudio](#cobaltspeech.cubic.RecognitionAudio)
    - [RecognitionConfig](#cobaltspeech.cubic.RecognitionConfig)
    - [RecognitionConfusionNetwork](#cobaltspeech.cubic.RecognitionConfusionNetwork)
    - [RecognitionResponse](#cobaltspeech.cubic.RecognitionResponse)
    - [RecognitionResult](#cobaltspeech.cubic.RecognitionResult)
    - [RecognizeRequest](#cobaltspeech.cubic.RecognizeRequest)
    - [StreamingRecognizeRequest](#cobaltspeech.cubic.StreamingRecognizeRequest)
    - [VersionResponse](#cobaltspeech.cubic.VersionResponse)
    - [WordInfo](#cobaltspeech.cubic.WordInfo)
  
    - [RecognitionConfig.Encoding](#cobaltspeech.cubic.RecognitionConfig.Encoding)
  
  
    - [Cubic](#cobaltspeech.cubic.Cubic)
  

- [Scalar Value Types](#scalar-value-types)



<a name="cubic.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## cubic.proto



<a name="cobaltspeech.cubic.ConfusionNetworkArc"></a>

### ConfusionNetworkArc
An Arc inside a Confusion Network Link


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| word | [string](#string) |  | Word in the recognized transcript |
| confidence | [double](#double) |  | Confidence estimate between 0 and 1. A higher number represents a higher likelihood that the word was correctly recognized. |






<a name="cobaltspeech.cubic.ConfusionNetworkLink"></a>

### ConfusionNetworkLink
A Link inside a confusion network


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| start_time | [google.protobuf.Duration](#google.protobuf.Duration) |  | Time offset relative to the beginning of audio received by the recognizer and corresponding to the start of this link |
| duration | [google.protobuf.Duration](#google.protobuf.Duration) |  | Duration of the current link in the confusion network |
| arcs | [ConfusionNetworkArc](#cobaltspeech.cubic.ConfusionNetworkArc) | repeated | Arcs between this link |






<a name="cobaltspeech.cubic.ListModelsRequest"></a>

### ListModelsRequest
The top-level message sent by the client for the `ListModels` method.






<a name="cobaltspeech.cubic.ListModelsResponse"></a>

### ListModelsResponse
The message returned to the client by the `ListModels` method.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| models | [Model](#cobaltspeech.cubic.Model) | repeated | List of models available for use that match the request. |






<a name="cobaltspeech.cubic.Model"></a>

### Model
Description of a Cubic Model


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [string](#string) |  | Unique identifier of the model. This identifier is used to choose the model that should be used for recognition, and is specified in the `RecognitionConfig` message. |
| name | [string](#string) |  | Model name. This is a concise name describing the model, and maybe presented to the end-user, for example, to help choose which model to use for their recognition task. |
| attributes | [ModelAttributes](#cobaltspeech.cubic.ModelAttributes) |  | Model attributes |






<a name="cobaltspeech.cubic.ModelAttributes"></a>

### ModelAttributes
Attributes of a Cubic Model


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| sample_rate | [uint32](#uint32) |  | Audio sample rate supported by the model |






<a name="cobaltspeech.cubic.RecognitionAlternative"></a>

### RecognitionAlternative
A recognition hypothesis


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transcript | [string](#string) |  | Text representing the transcription of the words that the user spoke. |
| confidence | [double](#double) |  | Confidence estimate between 0 and 1. A higher number represents a higher likelihood of the output being correct. |
| words | [WordInfo](#cobaltspeech.cubic.WordInfo) | repeated | A list of word-specific information for each recognized word. This is available only if `enable_word_confidence` or `enable_word_time_offsets` was set to `true` in the `RecognitionConfig`. |






<a name="cobaltspeech.cubic.RecognitionAudio"></a>

### RecognitionAudio
Audio to be sent to the recognizer


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| data | [bytes](#bytes) |  |  |






<a name="cobaltspeech.cubic.RecognitionConfig"></a>

### RecognitionConfig
Configuration for setting up a Recognizer


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| model_id | [string](#string) |  | Unique identifier of the model to use, as obtained from a `Model` message. |
| audio_encoding | [RecognitionConfig.Encoding](#cobaltspeech.cubic.RecognitionConfig.Encoding) |  | Encoding of audio data sent/streamed through the `RecognitionAudio` messages. For encodings like WAV/MP3 that have headers, the headers are expected to be sent at the beginning of the stream, not in every `RecognitionAudio` message.  If not specified, the default encoding is RAW_LINEAR16. <br> Depending on how they are configured, server instances of this service may not support all the encodings enumerated above. They are always required to accept RAW_LINEAR16. If any other `Encoding` is specified, and it is not available on the server being used, the recognition request will result in an appropriate error message. |
| idle_timeout | [google.protobuf.Duration](#google.protobuf.Duration) |  | Idle Timeout of the created Recognizer. If no audio data is received by the recognizer for this duration, ongoing rpc calls will result in an error, the recognizer will be destroyed and thus more audio may not be sent to the same recognizer. The server may impose a limit on the maximum idle timeout that can be specified, and if the value in this message exceeds that serverside value, creating of the recognizer will fail with an error. |
| enable_word_time_offsets | [bool](#bool) |  | This is an optional field. If this is set to true, the top result will include a list of words and the start time offset (timestamp) and the duration for each of those words. If set to `false`, no word-level timestamps will be returned. The default is `false`. |
| enable_word_confidence | [bool](#bool) |  | This is an optional field. If this is set to true, the top result will include a list of words and the confidence for those words. If `false`, no word-level confidence information is returned. The default is `false`. |
| enable_raw_transcript | [bool](#bool) |  | This is an optional field. If this is set to true, the transcripts will be presented as raw output from the recognizer without any formatting rules applied. They will be in all UPPER CASE, numbers and other special entities would be presented as the spoken words. If set to `false`, formatting rules will be applied to all results. The default is `false`. <br> As an example, if the spoken utterance was `here are four words`: with this field set to `false`: &#34;Here are 4 words&#34; with this field set to &#39;true&#39; : &#34;HERE ARE FOUR WORDS&#34; |
| enable_confusion_network | [bool](#bool) |  | This is an optional field. If this is set to true, the results will include a confusion network. If set to `false`, no confusion network will be returned. The default is `false`. If the model being used does not support a confusion network, results may be returned without a confusion network available. If this field is set to `true`, then `enable_raw_transcript` is also forced to be true. |






<a name="cobaltspeech.cubic.RecognitionConfusionNetwork"></a>

### RecognitionConfusionNetwork
Confusion network in recognition output


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| links | [ConfusionNetworkLink](#cobaltspeech.cubic.ConfusionNetworkLink) | repeated |  |






<a name="cobaltspeech.cubic.RecognitionResponse"></a>

### RecognitionResponse
Collection of sequence of recognition results in portion of an audio


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| results | [RecognitionResult](#cobaltspeech.cubic.RecognitionResult) | repeated |  |






<a name="cobaltspeech.cubic.RecognitionResult"></a>

### RecognitionResult
A recognition result corresponding to a portion of audio


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| alternatives | [RecognitionAlternative](#cobaltspeech.cubic.RecognitionAlternative) | repeated | An n-best list of recognition hypotheses alternatives |
| is_partial | [bool](#bool) |  | If this is set to true, it denotes that the result is an interim partial result, and could change after more audio is processed. If unset, or set to false, it denotes that this is a final result and will not change. <br> Servers are not required to implement support for returning partial results, and clients should generally not depend on their availability. |
| cnet | [RecognitionConfusionNetwork](#cobaltspeech.cubic.RecognitionConfusionNetwork) |  | If `enable_confusion_network` was set to true in the `RecognitionConfig`, and if the model supports it, a confusion network will be available in the results. |






<a name="cobaltspeech.cubic.RecognizeRequest"></a>

### RecognizeRequest
The top-level message sent by the client for the `Recognize` method.  Both
the `RecognitionConfig` and `RecognitionAudio` fields are required.  The
entire audio data must be sent in one request.  If your audio data is larger,
please use the `StreamingRecognize` call..


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| config | [RecognitionConfig](#cobaltspeech.cubic.RecognitionConfig) |  | Provides configuration to create the recognizer. |
| audio | [RecognitionAudio](#cobaltspeech.cubic.RecognitionAudio) |  | The audio data to be recognized |






<a name="cobaltspeech.cubic.StreamingRecognizeRequest"></a>

### StreamingRecognizeRequest
The top-level message sent by the client for the `StreamingRecognize`
request.  Multiple `StreamingRecognizeRequest` messages are sent. The first
message must contain a `RecognitionConfig` message only, and all subsequent
messages must contain `RecognitionAudio` only.  All `RecognitionAudio`
messages must contain non-empty audio.  If audio content is empty, the server
may interpret it as end of stream and stop accepting any further messages.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| config | [RecognitionConfig](#cobaltspeech.cubic.RecognitionConfig) |  |  |
| audio | [RecognitionAudio](#cobaltspeech.cubic.RecognitionAudio) |  |  |






<a name="cobaltspeech.cubic.VersionResponse"></a>

### VersionResponse
The message sent by the server for the `Version` method.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cubic | [string](#string) |  | version of the cubic library handling the recognition |
| server | [string](#string) |  | version of the server handling these requests |






<a name="cobaltspeech.cubic.WordInfo"></a>

### WordInfo
Word-specific information for recognized words


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| word | [string](#string) |  | The actual word in the text |
| confidence | [double](#double) |  | Confidence estimate between 0 and 1. A higher number represents a higher likelihood that the word was correctly recognized. |
| start_time | [google.protobuf.Duration](#google.protobuf.Duration) |  | Time offset relative to the beginning of audio received by the recognizer and corresponding to the start of this spoken word. |
| duration | [google.protobuf.Duration](#google.protobuf.Duration) |  | Duration of the current word in the spoken audio. |





 


<a name="cobaltspeech.cubic.RecognitionConfig.Encoding"></a>

### RecognitionConfig.Encoding
The encoding of the audio data to be sent for recognition.

For best results, the audio source should be captured and transmitted using
the RAW_LINEAR16 encoding.

| Name | Number | Description |
| ---- | ------ | ----------- |
| RAW_LINEAR16 | 0 | Raw (headerless) Uncompressed 16-bit signed little endian samples (linear PCM), single channel, sampled at the rate expected by the chosen `Model`. |
| WAV | 1 | WAV (data with RIFF headers), with data sampled at a rate equal to or more than the sample rate expected by the chosen `Model`. If the WAV data has more than one channels, only the first channel will be used for recognition. |
| MP3 | 2 | MP3 data, sampled at a rate equal to or more than the sampling rate expected by the chosen `Model`. If the MP3 data has more than one channels, only the first channel will be used for recognition. |


 

 


<a name="cobaltspeech.cubic.Cubic"></a>

### Cubic
Service that implements the Cobalt Cubic Speech Recognition API

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| Version | [.google.protobuf.Empty](#google.protobuf.Empty) | [VersionResponse](#cobaltspeech.cubic.VersionResponse) | Queries the Version of the Server |
| ListModels | [ListModelsRequest](#cobaltspeech.cubic.ListModelsRequest) | [ListModelsResponse](#cobaltspeech.cubic.ListModelsResponse) | Retrieves a list of available speech recognition models |
| Recognize | [RecognizeRequest](#cobaltspeech.cubic.RecognizeRequest) | [RecognitionResponse](#cobaltspeech.cubic.RecognitionResponse) | Performs synchronous speech recognition: receive results after all audio has been sent and processed. It is expected that this request be typically used for short audio content: less than a minute long. For longer content, the `StreamingRecognize` method should be preferred. |
| StreamingRecognize | [StreamingRecognizeRequest](#cobaltspeech.cubic.StreamingRecognizeRequest) stream | [RecognitionResponse](#cobaltspeech.cubic.RecognitionResponse) stream | Performs bidirectional streaming speech recognition. Receive results while sending audio. This method is only available via GRPC and not via HTTP&#43;JSON. However, a web browser may use websockets to use this service. |

 



## Scalar Value Types

| .proto Type | Notes | C++ Type | Java Type | Python Type |
| ----------- | ----- | -------- | --------- | ----------- |
| <a name="double" /> double |  | double | double | float |
| <a name="float" /> float |  | float | float | float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long |
| <a name="bool" /> bool |  | bool | boolean | boolean |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str |

