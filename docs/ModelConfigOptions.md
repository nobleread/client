### Config Option Documentation


#### Debug options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| debug.save_lattices | Whether we save a lattice | bool | 0 |
| debug.save_lattices_path | Directory where we save the lattices | path string | "" |



#### Endpoint options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| endpoint.type | Type of endpointer ("all_speech", "rule_based", "kaldi_online", or "keyword") | string | "all_speech" |
| endpoint.silence_phone_string | String of silence phonemes | string | "" |
| endpoint.must_reach_final | Whether utterance must be in a final state to be endpointed | bool | 1 |
| endpoint.must_contain_non_silence | Whether utterance must contain non-silence to be endpointed | bool | 1 |
| endpoint.trailing_silence_frames | Minimum number of trailing silence frames to endpoint | int | 10 |
| endpoint.max_relative_cost | Maximum relative cost | float | 100 |
| endpoint.mininum_utterance_length_frames | Minimum length of an utterance before we end-point | int | 10 |
| endpoint.max_silence_frames | Maximum number of allowed trailing silence frames before endpoint, used only if endpointer.type is keyword | int | 75 |
| endpoint.kaldi_online_endpointer_config | Filepath for kaldi's online endpointer configs, used only if endpointer.type is kaldi-online. | path string | "" |



#### Front End Options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| fe.acoustic_sampling_rate_hz | Sampling rate for audio, the model should be tuned to this sampling rate. Behavior is undefined if audio of mis-matched sampling rate is pushed to the engine. 8000 means 8kHz. | float | 8000 |
| fe.input_sampling_rate_hz | Sampling rate of the incoming audio; default 0 means no resampling needed | float | 0 |
| fe.frame_shift_ms | Frame shift in milliseconds | float | 10.0 |
| fe.frame_length_ms | Frame length in milliseconds | float | 25.0 |
| fe.dither | Dithering amount, use 0.0 to disable dithering | float | 1.0 |
| fe.preemph_coeff | Preemphasis Coefficient | float | 0.97 |
| fe.remove_dc_offset | If true, subtract the wave mean before taking the FFT | bool | 1 |
| fe.round_to_pow_2 | If true, round the window size up to the next power of 2 | bool | 1 |
| fe.blackman_coeff | Blackman Coefficient to use | float | 0.42 |
| fe.snip_edges | If true, fit all frames completely into the waveform | bool | 1 |
| fe.window_type | What windowing algorithm to use. May be "hamming", "rectangular", "povey", "hanning", or "blackman" | string | "povey" |
| fe.num_cepstrals | Number of cepstra in MFCC computation (including C0) | int | 13 |
| fe.use_energy | Use energy (not C0) in MFCC computation | bool | 1 |
| fe.cepstral_lifter | Constant that controls scaling of features | float | 22.0 |
| fe.num_mel_bins | Number of triangular mel-frequency bins | int | 23 |
| fe.mel_low_frequency | Low cutoff frequency for mel bins | float | 20.0 |
| fe.mel_high_freq | High cutoff frequency for mel bins (if \< 0, offset from Nyquist) | float | 0.0 |
| fe.feature_type | Type of feature. May be "plp", "mfcc", or "fbank" | string | "mfcc" |
| fe.mel_energy_floor | Energy floor taking logs in mel energy, 0 to set it to std::numeric_limits\<float>::min() value | float | 0 |
| fe.lpc_order | Order of LPC analysis in PLP computation | int | 12 |
| fe.compress_factor | Compression factor in PLP computation | float | 0.333333 |
| fe.cepstral_scale | Scaling constant in PLP computation | float | 1.0 |
| fe.use_log_fbank | If true, produce log-filterbank, else produce linear. | bool | 1 |
| fe.use_power | If true, use power, else use magnitude | bool | 1 |
| fe.random_generator_seed | The seed value for generating random numbers | int | 0 |
| fe.random_pool_size | The number of random ints that will be generated using the random number seed | int | 50000 |
| fe.use_energy_vad | Use energy based voice activity detection | bool | 0 |
| fe.vad_energy_threshold | Constant term in energy threshold for MFCC0 for VAD (also see fe.vad_energy_mean_scale) | float | 5.0 |
| fe.vad_energy_mean_scale | The actual threshold is calculated as `vad_energy_mean_scale*m + vad_energy_threshold` where `m` is the mean log-energy of the file  | float | 0.5 |
| fe.vad_frame_context | Number of frames of context on each side of central frame, in window for which energy is monitored | int | 5 |
| fe.vad_proportion_threshold | Parameter controlling the proportion of frames within the window that need to have more energy than the threshold | float | 0.6 |
| fe.vad_min_event_threshold | Length in ms of silence or speech to see before event is created | int | 300 |
| fe.use_start_pointer | Use energy vad output to gate what frames are processed | bool | 0 |
| fe.ivector-extraction-config | i-vector extraction config | path string | "" |
| fe.ivector_extractor_info.use_most_recent_ivector | Uses the most recent i-vector available when set. May slightly improve accuracy but has the drawback results will be non-deterministic because features will change based on how fast audio was pushed.  Use only when WER gain is significant. | bool | 0 |
| fe.ivector_use_ivectors | Enable iVector use for adaptation | bool | 0 |
| fe.ivector_extractor_info.greedy_ivector_extractor | If true, use greedy iVector extraction | bool | 0 |
| fe.ivector_lda_matrix | Filename of LDA matrix, e.g. final.mat; used for iVector extraction. | path string | "" |
| fe.ivector_global_cmvn_stats | (Extended) filename for global CMVN stats only used for iVector extraction | path string | "" |
| fe.ivector_cmvn_config | Configuration file for online CMVN features only used for iVector extraction | path string | "" |
| fe.ivector_splice_config | Configuration file for frame splicing only used for iVector extraction | path string | "" |
| fe.ivector_diag_ubm | Filename of diagonal UBM used to obtain posteriors for iVector extraction | path string | "" |
| fe.ivector_extractor | Filename of iVector extractor | path string | "" |
| fe.ivector_period | Frequency with which we extract iVectors for neural network adaptation | int | 10 |
| fe.ivector_num_gselect | Number of Gaussians to select for iVector extraction | int | 5 |
| fe.ivector_min_post | Threshold for posterior pruning in iVector extraction | float | 0.025 |
| fe.ivector_posterior_scale | Scale for posteriors in iVector extraction (may be viewed as inverse of prior scale) | float | 0.1 |
| fe.ivector_max_count | Maximum data count we allow before we start scaling the stats down (if nonzero). | float | 100 |
| fe.ivector_use_most_recent_ivector | Always use most recent available i-vector, rather than the one for the designated frame.  Use with care as it leads to non-deterministic behavoir because features change with the audio delivery rate. | bool | 0 |
| fe.ivector_greedy_extractor | 'read ahead' as many frames as we currently have available when extracting the i-vector. | bool | 0 |
| fe.ivector_max_remembered_frames | The maximum number of frames of adaptation history that we carry through o later utterances of the same speaker. Set to 0 to remember all frames. Remembering all frames may be necessary to resolve issues when fe.use_silence_weighting is active. | int | 1000 |
| fe.use_silence_weighting | Enable silence weighting for adaptation.  Requires fe.ivector_use_ivector=1. | bool | 0 |
| fe.ivector_frame_limit | The number of frames at the beginning of a stream that will be used for i-vector estimation. After this number of frames is reached the i-vector will remain contant for the rest of the audio stream.  A value of -1 will use all frames in the stream, and a value of 0 will force all i-vectors to contain only zero values. | int | -1 |
| fe.ivector_skip_period | If set (value is >1), only 1 of every fe.ivector_skip_period frames are used to accumulate and potentially estimate i-vectors. When this parameter is used fe.ivector_period should be a multiple of fe.ivector_skip_period or the i-vector will be re-estimated less than expected.  If both fe.ivector_frame_limit is >0 and fe.ivector_skip_period is set (>1), the i-vector frame skipping only happens after the first fe.ivector_frame_limit are processed. | int | -1 |
| fe.silence_weighting_phones | List of integer ids of silence phones, separated by colons (or commas).  Data that (according to the traceback of the decoder) corresponds to these phones will be downweighted by fe.silence_weighting_weight. | string | "" |
| fe.silence_weighting_weight | Weighting factor for frames that the decoder trace-back identifies as silence; only relevant if the fe.silence_weighting_phones option is set. | float | 1.0 |
| fe.silence_weighing_max_state_duration | Maximum allowed duration of a single transition-id; runs with durations longer than this will be weighted down to the silence-weight. | float | -1 |
| fe.get_pitch | If true, extract pitch, NCCF pairs for each frame. | bool | 0 |
| fe.min_pitch | Minimum pitch (F0) to search for, in Hz. | float | 50 |
| fe.max_pitch | Maximum pitch (F0) to search for, in Hz. | float | 400 |
| fe.soft_min_pitch | Minimum pitch (F0), applied in soft way. Must not exceed fe.min_pitch. | float | 10 |
| fe.penalty_factor | Cost factor for F0 change. | float | 0.1 |
| fe.lowpass_cutoff | Cufoff frequency for lowpass filter, in Hz. | float | 1000 |
| fe.resample_freq | Frequency that we downsample the signal to. Must be more than twice the lowpass-cutoff. | float | 4000 |
| fe.delta_pitch | Smallest relative change in pitch that our algorithm measures | float | 0.005 |
| fe.nccf_ballast | Increasing this factor reduces NCCF for quiet frames | float | 7000 |
| fe.lowpass_filter_width | Integer that determines filter width of lowpass filter, more gives sharper filter | int | 1 |
| fe.upsample_filter_width | Integer that determines filter width when upsampling NCCF | int | 5 |
| fe.max_frames_latency | Max number of frames of latency to allow pitch tracking to introduce into the feature processing | int | 0 |



#### Recognizer options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| model.type | The type of model, default is asr (options are "asr" or "keyword" | string | "asr" |
| recognizer.log_trace | If true, print the trace of the recognizer's steps to the logger | bool | 0 |
| recognizer.long_running | Set to 1 if the recognizer is expected to process more than a few minutes of audio.  This option makes the metrics timeline track aggregates instead of individual events so memory usage is bounded. | bool | 0 |
| recognizer.dump_audio_path | Path where recognizers should save audio input for use in troubleshooting, empty disables dumping audio | path string | "" |



#### Results options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| results.partial_results.emit_frequency_frames | When set to a positive number N, the decoder will look for a partial result every N frames.  Partial results is off if set to <= 0. | int | -1 |
| results.nbest_generation_strategy | nbest generation algorithm. May be empty or 'lattice'. If it is empty, we will not generate an nbest. If it is 'lattice', we will generate the nbest from the lattice. | string | "" |
| results.nbest | Nbest value; if set to N, generate 0-N results | int | 1 |
| results.word_boundary_filepath | file needed to align words with audio frames in order to obtain timestamps (word_boundary.int) | path string | "" |
| results.build_cnet | If true, build the confusion network | bool | 0 |
| results.emit_cnet | If true, include the cnet in the output | bool | 0 |
| results.mbr_decoding | If true, do the mbr_decoding when creating the cnet | bool | 1 |
| results.nbest_results_type | Should we emit the nbest results or not; null string for no nbest result; cnet for building from cnet | string | "" |
| results.time_align_cnet | If true, align the compact lattice for cnet building | bool | 1 |
| results.calculate_perplexity | If true, calculate perplexity | bool | 0 |
| results.perplexity_fst_filepath | filepath for the fst used to calculate perplexity | path string | "" |
| results.max_mem | Max memory used in lattice determinization | int | 5000000 |
| results.emit_metrics | If true, emit metrics for this recognizer | bool | 1 |
| results.hmm_input_symbol_path | filepath for text based input symbol table of HMM (HCLG) | path string | "" |
| results.emit_clat_result |  If true, emit a result from the clat (which contains info about phoneme timestamps, but not confidence) | bool | 0 |
| results.hmm_input_symbol_path | filepath for text based input symbol table of HMM (HCLG) | path string | "" |
| results.collect_energy |  If true, include the energy from all frames in the results payload | bool | 0 |
| results.collect_pitch |  If true, include the pitch from all frames in the results payload | bool | 0 |
| results.collect_posteriors | If true, include the AM posteriors from all frames in the results payload | bool | 0 |



#### Scorer options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| am-compatibility-id | The parameter that tells us the compatibility of different models for loading speaker adaptation stats. If the compatibility info of the adaptation and the model matches, we should be able to load the adaptation stats for our model. The id-giving process should be done carefully to avoid naming collisions | string | "unknown" |
| scorer.scorer_type | type of scorer we use during acoustic scoring | string | "kaldi-mlp" |
| scorer.quantization_seg_length | Length of the quantized segment for quantized_segmented scorer to use | int | 4 |
| scorer.acoustic_model_filepath | filepath of the acoustic model to use for scoring | path string | "" |
| scorer.acoustic-scale | scales the acoustic models scores by this number | float | 0.1 |
| scorer.skip_num_frames | The number of batch size for frames for which only one will be scored; negative number to disable frame skipping | int | -1 |
| scorer.frame_subsampling_factor | Frame subsampling factor. (Will usually be 1 except for chain models) | int | 1 |
| scorer.extra_left_context_initial | Extra left context to use at the first frame of an utterance (note: this will just consist of repeats of the first frame, and should not usually be necessary) | int | 0 |
| scorer.frames_per_chunk | Number of frames in each chunk that is separately evaluated by the neural net.  Measured before any subsampling, if the --frame-subsampling-factor options is used (i.e. counts input frames).  This is only advisory (may be rounded up if needed). | int | 20 |



#### Search options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| search.decode_fst_type | Type of decode-fst. | string | "vector_fst" |
| search.decode_fst_filepath | filepath of the fst to use for decoding (HCLG.fst) | path string | "" |
| search.decoder_type | type of decoder to use during search | string | "kaldi-lattice" |
| search.output_symbol_table_filepath | filepath to output symbol table | path string | "" |
| search.max_active | maximum number of toks during decoding, enforced between PNE and PE | float | 7000 |
| search.beam | decoder's beam | float | 15.0 |
| search.lattice_beam | result building lattice beam | float | 6.0 |
| search.hcl_fst_filepath | filepath of the HCL fst for decoding (HCL.fst), to be used with dynamic decoding | path string | "" |
| search.g_fst_filepath | filepath of the fst we'll use for decoding (G.fst), to be used with dynamic decoding | path string | "" |
| search.on_separate_thread | If true, perform search work on a separate thread from the scoring work | bool | 0 |



#### Speech Verification options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| speech_verification.lexicon_filepath | filepath to lexicon for speech verification | path string | "" |
| speech_verification.vowels_filepath | filepath to list of vowel phones for speech verification | path string | "" |
| speech_verification.align_vowels | If true, favor aligning vowels in the reference to vowels in the hypothesis list. Should also set vowels_filepath. | bool | 0 |
| speech_verification.max_candidate_prons | max number of prons to consider for speech verification | int | 250 |
| speech_verification.cutoff_filepath | filepath to per-pron thresholds (the confidence threshold for the pron being 'correct') for speech verification | path string | "" |
| speech_verification.stress_regressor_filepath | filepath to SVM model used to score stress | path string | "" |
| speech_verification.svm_scale_filepath | filepath to scale used on input features to the SVM model | path string | "" |
| speech_verification.output_svm_feature_vector | If true, output feature vectors for stress scoring | bool | 0 |
| speech_verification.cnet_link_threshold | The combined scores of the candidates in a link must exceed this threshold to be included in the alignment | float | 0.0 |
| speech_verification.noise_phone_string | Colon-delimited list of phone IDs (corresponding to output symbol IDs) to exclude in alignment | string | "" |



#### ASR model specific options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| results.emit_confidence | If true, emit word level confidences for nbest result list | bool | 0 |
| results.tokens_nbest_redundant | The CSV list which contains strings that are considered redundant for the purpose of nbest results | string | "\<eps>,[noise],\<s>" |
| results.emit_timestamps | Whether to produce word timestamps | bool | 0 |
| results.scale_speech_posteriors | If true, scale up speech posteriors for better results with limited grammars and very similar phrases | bool | 0 |
| results.nonspeech_tokens | The csv list which contains token strings that are considered to be non-speech | string | "\<eps>,\#0,\<VOCAL_NOISE>,\<NON_SPEECH_NOISE>,SIL,~SIL,sil,~sil" |



#### Keyword model specific options
| Option | Description | Type | Default Value |
| --- | --- | --- | --- |
| keyword.g2p_model | Path to the grapheme-to-phoneme fst | path string | "" |
| keyword.word_symbol_table | Path to the word symbol table | path string | "" |
| keyword.phone_symbol_table | Path to the phone symbol table | path string | "" |
| keyword.context_dependency | Path to the context dependency (tree) file | path string | "" |
| keyword.lexicon | Path to the context dependency lexicon text file | path string | "" |
| keyword.lm_keyword_cost | Cost (neg-log-prob) of taking the keyword path | float | 0.0 |
| keyword.lm_background_cost | Cost (neg-log-prob) of taking the background path | float | 0.0 |
| keyword.lm_background_tokens | Comma-separated list of background word tokens (e.g. '\<UNK>,\<VOCAL_NOISE>') | string | "" |
| keyword.lex_sil_prob | Probability of optional silence in the lexicon | float | 0.5 |
| keyword.lex_sil_phone | Silence phone token | string | "SIL" |
| keyword.confidence_cost_map | Path to file containing JSON object that describes mappings from confidence values to keyword path costs | path string | "" |
| results.emit_confidence | Should we enable emitting word level confidences for nbest result list? | bool | 1 |
| results.tokens_nbest_redundant | The CSV list which contains strings that are considered redundant for the purpose of nbest results | string | "\<eps>,[noise],\<s>,\<unk>" |
| results.emit_timestamps | Whether to produce word timestamps | bool | 1 |
| results.scale_speech_posteriors | Should speech posteriors be scaled up for better results with limited grammars and very similar phrases? | bool | 1 |
| results.nonspeech_tokens | The csv list which contains token strings that are considered to be non-speech | string | "\<UNK>,\<eps>,\#0,\<VOCAL_NOISE>,\<NON_SPEECH_NOISE>,SIL,~SIL,sil,~sil" |
