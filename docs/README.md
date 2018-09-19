## Overview

The Cubic gRPC Server only defines 4 API functions.  

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| Version | [.google.protobuf.Empty](API.md#google.protobuf.Empty) | [VersionResponse](API.md#cobaltspeech.cubic.VersionResponse) | Queries the Version of the Server |
| ListModels | [ListModelsRequest](API.md#cobaltspeech.cubic.ListModelsRequest) | [ListModelsResponse](API.md#cobaltspeech.cubic.ListModelsResponse) | Retrieves a list of available speech recognition models |
| Recognize | [RecognizeRequest](API.md#cobaltspeech.cubic.RecognizeRequest) | [RecognitionResponse](API.md#cobaltspeech.cubic.RecognitionResponse) | Performs synchronous speech recognition: receive results after all audio has been sent and processed. It is expected that this request be typically used for short audio content: less than a minute long. For longer content, the `StreamingRecognize` method should be preferred. |
| StreamingRecognize | [StreamingRecognizeRequest](API.md#cobaltspeech.cubic.StreamingRecognizeRequest) stream | [RecognitionResponse](API.md#cobaltspeech.cubic.RecognitionResponse) stream | Performs bidirectional streaming speech recognition. Receive results while sending audio. This method is only available via GRPC and not via HTTP&#43;JSON. However, a web browser may use websockets to use this service. |

## Object Hierarchy

The following shows the relationships between each object defined in the API requests and responses.  Further information on each item is defined in the [API document](API.md).

1. [Version](API.md#cobaltspeech.cubic.Cubic)
    - [VersionResponse](API.md#cobaltspeech.cubic.VersionResponse)
2. [ListModels](API.md#cobaltspeech.cubic.Cubic)
    - [ListModelsRequest](API.md#cobaltspeech.cubic.ListModelsRequest)
    - [ListModelsResponse](API.md#cobaltspeech.cubic.ListModelsResponse)
        - [Model](API.md#cobaltspeech.cubic.Model)
            - [ModelAttributes](API.md#cobaltspeech.cubic.ModelAttributes)
3. [Recognize](API.md#cobaltspeech.cubic.Cubic)
    - [RecognizeRequest](API.md#cobaltspeech.cubic.RecognizeRequest)
        - [RecognitionConfig](API.md#cobaltspeech.cubic.RecognitionConfig)
            - [RecognitionConfig.Encoding](API.md#cobaltspeech.cubic.RecognitionConfig.Encoding)
        - [RecognitionAudio](API.md#cobaltspeech.cubic.RecognitionAudio)
    - [RecognitionResponse](API.md#cobaltspeech.cubic.RecognitionResponse)
        - [RecognitionResult](API.md#cobaltspeech.cubic.RecognitionResult)
            - [RecognitionAlternative](API.md#cobaltspeech.cubic.RecognitionAlternative)
                - [WordInfo](API.md#cobaltspeech.cubic.WordInfo)
            - [RecognitionConfusionNetwork](API.md#cobaltspeech.cubic.RecognitionConfusionNetwork)
                - [ConfusionNetworkLink](API.md#cobaltspeech.cubic.ConfusionNetworkLink)
                    - [ConfusionNetworkArc](API.md#cobaltspeech.cubic.ConfusionNetworkArc)
4. [StreamingRecognize](API.md#cobaltspeech.cubic.Cubic)
    - [StreamingRecognizeRequest](API.md#cobaltspeech.cubic.StreamingRecognizeRequest)
        - [RecognitionConfig](API.md#cobaltspeech.cubic.RecognitionConfig)
            - [RecognitionConfig.Encoding](API.md#cobaltspeech.cubic.RecognitionConfig.Encoding)
        - [RecognitionAudio](API.md#cobaltspeech.cubic.RecognitionAudio)
    - [RecognitionResponse](API.md#cobaltspeech.cubic.RecognitionResponse)
        - [RecognitionResult](API.md#cobaltspeech.cubic.RecognitionResult)
            - [RecognitionAlternative](API.md#cobaltspeech.cubic.RecognitionAlternative)
                - [WordInfo](API.md#cobaltspeech.cubic.WordInfo)
            - [RecognitionConfusionNetwork](API.md#cobaltspeech.cubic.RecognitionConfusionNetwork)
                - [ConfusionNetworkLink](API.md#cobaltspeech.cubic.ConfusionNetworkLink)
                    - [ConfusionNetworkArc](API.md#cobaltspeech.cubic.ConfusionNetworkArc)
