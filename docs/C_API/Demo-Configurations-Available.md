The following configurations are currently available in the demo:

| Identifier | Configuration |
|------------|---------------|
| (A) US English (16KHz WAV audio)|available in online demo|
| (B) US English (8KHz WAV audio)|available in online demo|
| asr-model  | default for on-premises demo       |
| asr-model-with-stats | outputs confusion network (on premises) |
| spanish-asr-model | Spanish ASR (on premises) |



You can switch between configurations using the --list_file parameter of the client.  For example:
```
python ./py/client.py  --url <endpoint> --list_file True ./audio/hello_world.list
```

The file referenced by the --list_file parameter (./audio/hello_world.list in this case) has the following json form:
```
{"audio_path": "hello_world_16khz.wav", "model_id":"asr-model-with-stats"}
```

The "model_id" field identifies which of the above configurations to use.