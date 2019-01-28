# Deprecated C API
### NOTE: this C API has been deprecated in favor of the [gRPC API](https://github.com/cobaltspeech/client).  This documentation is provided to support legacy clients.

This document provides an overview of the API for the Cobalt ASR engine. We begin with an overview of the tenets of the API, tour a few API entry points, proceed to show an example workflow, and wrap up by discussing the result object model.

**Api tenets:**

This is a thread-safe speech recognition API.

The objective of this API is to convert speech to text.

There are two main types of 'objects' in this API: Models and Recognizers.

A Model is an encapsulation for machine-learned artifacts that were trained in offline, supervised fashion. Models are the 'brain' of our system. Models own the accuracy of recognition.

A Recognizer is created from a model and contains the logic for performing speech recognition. Recognizers own the latency and stability of recognition.

Models are typically heavy weight objects that should be created once and used often. Recognizers are light weight objects should be created per audio stream (unless otherwise advised).

This API provides two main classes of functions: API level functions, prefixed by API_, are responsible for creating and deleting objects, such as models and recognizers. Object specific functions, prefixed by object names such as Recognizer_, are responsible for controlling specific functionality of each object, such as pushing audio to a recognizer.

Each API call returns success or failure. If the call is successful, the object created for the API call may be used; if the call failed, the reason for failure is documented in the return struct.

The API is stateful, and uses string (char*) based IDs to maintain state; e.g. clients create a Models and control the ID of the model by setting modelIdC, that model may then be used by calling other API functions with the same ID.

Memory management is crucial to the success of the API, the client must make sure each pointer object passed to an API call remains in scope until the call returns. Pointers returned by the API will remain in scope until the client calls the "clear" function. Best practice is to copy the contents of each pointer, and call clear() immediately, to avoid memory usage growth.

The API delivers objects via callbacks, e.g. logging, metrics, and results.

Callbacks are registered as function pointers. The registration calls are not thread-safe. Best practice is to registered each callback once at the start of the process. Callbacks should be registered prior to any other API calls.

The API delivers per-recognition results in asynchronous fashion. To allow the client to map a result to a recognizer, the result (and metrics) callbacks contain a field for the id of the recognizer. Each recognizer object delivers (0+) results per lifetime, depending on the underlying model and Recognizer_ API calls.

**Tour of API Entry Points**

[cubic.h](https://github.com/cobaltspeech/cubic/blob/master/cpp/api/cubic.h)

**API workflow example**

[exampleWorkflow.cpp](https://github.com/cobaltspeech/cubic/blob/master/cpp/client/example_workflow.cpp)

**Recognizer construct**

In the New Recognizer call: Api_NewRecognizer(recognizerId.c_str(), modelId.c_str()), the second parameter may be a string representing a model-id, or a JSon string object that represents a model, a model-domain, a recognizer-type, and a transcript.

Here is the full JSon:

| key | value|
|---|---|
| modelset_id | identifies a modelset, required parameter if construct is JSon.|
| recognizer_type | type of recognizer, defaults to 'asr', optional.|
| model_domain | domain of model, identified by modelset_id, defaults to default domain, optional.|
| transcripts | required for models/recognizers like limited_vocab, optional for asr recognizers.|



**Result object model**

The result object model is JSON, and described in detail [[ Results-Object-Model.md ]]
