# TACOTRON: TOWARDS END-TO-END SPEECH SYN- THESIS

__note: vocoder__

* sequence to sequence framwork
* generate speech at the frame level, faster than sample-level autoregressive methods

* Modern text-to-speech pipelines(statistical parametric TTS for example):
    - text frontend
    - duration model
    - acoustic feature prediction model
    - signal-processing-based vocoder
    
* differences with end-to-end speech recognition and machine translation:
    - output sequences much longer than those of the output, which cause prediction errors to accumulate quickly

### related work
* wavenet is slow
    - sample-level autoregressive nature
    - coditioning on linguistic features from existing TTS frontend, not end-to-end
  
* deepvoice 
    - trained independently

### Model Architecture
* CBHG module: 
    - extracting representations from sequences
    - 1-D conv bank + maxpool along time + conv1D projections(+Residual connections) + Highwaylayers + bidirecitonal RNN.   __Inspired from machine translation Lee et al., 2016__
    
* encoder
    - goal: extract robust sequential representations of text
    
    - bottleneck layer with dropout as pre-net
    - CBHG module transform the prenet outputs into the final encoder
    

* decoder
    - use content-based tanh attention decoder ( Vinyals et al., 2015)
    - use stack of GRUs with residual connections for decoder ( speed up convergence)
    - decoder target: use 80-band mel-scale spectrogram, use post-processing network to convert from the seq2seq target to waveform. (reason:directly predict raw spectrogram is highly redundant representation)
    - trick: predicting multiple, non-overlapping frames at each decoder step
    
* post-processiong net and waveform synthesis
    - Griffin-Lin as the synthesizer
    - use CBHG module for the post-processing net-work