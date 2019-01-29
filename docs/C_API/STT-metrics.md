Cobalt’s recognizer emits a speech-to-text metric, named “stt_metrics”, for every recognition result. These metrics are designed to report and diagnose the latency of said recognition. 

**Latency measuring metrics**

Definition of Latency:

We define and measure latency as the difference in time from when the customer finished speaking, termed “end-of-speech”, to when we emitted the result. 

Metrics:

Below is a list of our metrics focused on measuring and breaking latency to actionable buckets.

| Metric Name | Description
| --- | ---
|stt_latency_msec | The engine’s best guess on the latency the customer experiences for this utterance.
|delivery_latency_msec | Amount of latency attributable to how the customer delivered audio.
|endpoint_latency_msec | Amount of latency attributable to detecting end-of-speech.


**Diagnostic metrics:**

In addition to the top level latency metrics, the engine publishes a set of metrics to aid in diagnosing or alarming on what causes abnormal latency values:

|Metric name	| Description
| --- | ---
| total_wall_clock_processing_msec	|Total time spent by the engine in processing audio.
| total_audio_processed_msec | 	Total amount of audio processed.
|scoring_time_ratio| Ratio of audio time to time spent evaluating the DNN acoustic model, multiplied by 1000 and normalized to integer space;  e.g. value of 2000 means the engine spent 2 seconds processing every second of audio.
|search_time_ratio | Same as scoring_time_ratio except this ratio measure time to execute the Viterbi search algorithm.
| cputime_audio_ratio | 	Ratio of audio to compute time.  It should be noted that this ratio tallies all time spent on all CPUs (even in parallel).  So when the model uses search.on_separate_thread to run the scoring and search on separate processors, this value will look much higher than it would with them on the same processor even though the overall latency introduced by processing time may be lower.
| result_building_latency_msec |	The amount of time spent building a result.
| recognizer_startup_cputime_msec |	The amount of time spent starting up the recognizer.
| callbacks_msec | The amount of time we spent *in* callbacks.
| audio_delivery_ratio | Ratio of the amount of audio processed and delivery time of that audio; e.g.  if the value is 2000, then 1 second of audio was delivered in 0.5 seconds.
| fe_processing_msec | The amount of time we spent in the frontend.
| active_recognizers_count | The number of recognizers active, when the current recognizer was constructed, includes the current recognizer.
| metrics_building_msec | The amount of time spent calculating the metrics values.
| wait_time_per_processor | A map of threaded processor name to how much time it spent waiting for data to process in milliseconds.

**Effect of previous utterances:**

We define all audio delivered via a singular customer interaction as a “stream”. This stream of audio may be broken up into several “utterances”. The state of recognition for previous utterances matter in our metrics for the current utterance: e.g. what happens in utterance-0 may carry over to utterance-1.

It is illustrative to consider a dummy example:

1.	We incur a latency of 3 seconds on utterance-0 due to high cputime_audio_ratio
2.	We have real-time processing, it takes us 1 second to process 1 second of audio, for utterance-1.
3.	Our latency for utterance-1 is still 3 seconds.

In this scenario, the engine never caught up to the audio being pushed, and the second utterance in the stream maintains the 3 second latency number, even though we were real-time processing the second utterance. 

**Metrics Object Model and emission:**

The engine emits metrics via a callback. The metrics are emitted in JSON form, with the following top-level format:


```
{
    "cobalt_object": "stt_metrics",
    "metrics": <see next section>
    "version": "1"
}
```

The “metrics” key points to a map<string, int> structure of metrics. The metrics are:


```
    "metrics": {
        "callbacks_msec": 0,
        "cputime_audio_ratio": 723,
        "recognizer_startup_cputime_msec": 55,
        "result_building_latency_msec": 2,
        "stt_latency_msec": 1358,
        "total_audio_processed_msec": 1936,
        "total_wall_clock_processing_msec": 1401
    },
```
