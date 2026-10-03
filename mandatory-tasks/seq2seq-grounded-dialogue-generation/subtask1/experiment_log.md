# Subtask_1 — Experiment Log

NOTE: My entire experiment and a lot of findings/readings Ive taken is based on the research paper
that i read - Seq2Seq Learning with nn (-Sutskever et al. 2014).

## GOAL

- Our baseline goal is to establish a simple baseline model for English-to-French translation.
- Implement an encoder-decoder structure from scratch without using any form of attention.

---

## Dataset/ Preprocessing

- English → French parallel corpus.
- 232,825 training pairs, 890 validation, 8,597 test.
- 10,000-word vocabulary for each language.
- <PAD>, <SOS>, <EOS>, <UNK> special tokens.
- Maximum sequence length = 50.
- Batch size = 64.

## Architecture

- RNN encoder.
- RNN decoder.
- Embedding dimension = 256.
- Hidden dimension = 512.
- Encoder compresses the source sentence into its final hidden state.
- Decoder uses that hidden state to generate the French sentence token by token.
- No attention.

## Model Size

- Encoder: 2,954,240 parameters
- Decoder: 8,084,240 parameters
- Total: 11,038,480 parameters

## Experiments (till now)

1. So basically ive worked woth RNNs/ LSTMs before and i know the main bottleneck of these 
seq-to-seq models is the context vector problem. So basically, after processing everything the
entire set of info is sent to the decoder through a context vector. So choosing the token 
length and vocab is crucial.

2. When I examined the dataset, I concluded choosing length = 50 tokens, batch_size = 64, and 
input vocab as 10,000. (Still training on 232,825) training pairs.

3. I kept the emb dim and hidden dim as 256 and 512 (the standard that we take for most models).

4. My next step ill try implementing teacher forcing (as it gives us better training results ofc).

## What you want us to investigate: 

- How well a basic fixed-context encoder-decoder translates.
- Whether it struggles with longer sentences.
- Whether rare/unknown words cause problems.

# EXPERIMENT 1

1. I firstly experimented on a cpu (by mistake) with 232k pairs and 11M para (it was not finishing lol) .
Then I switched to a T4 GPU, since I had experience using it before (IEEE DocForge project).

2. Then i tried for 1 epoch, but it took around 5 mins (too long). So i changed the fundamental way i was
applying teacher forcing. Intead of 1 at a time for all 48 tokens i tried processing all of them together
(not individually).

OBSERVATIONS : 

- RNN trained for 4 epochs.
- Training loss decreased/stabilized around 4.01.
- RNN BLEU score on the test set: 0.00286 (the observed behavior).
- Test sentences mostly produced the same output: et je pense que c'est un peu plus de <UNK> .
- The same output was also produced for different training examples, showing that the model was not effectively conditioning its prediction on the input sentence.
- <UNK> appeared in the generated output, indicating difficulty handling words outside the limited vocabulary.
- The results show the limitation of a basic RNN encoder-decoder with a fixed-length context, especially for longer sentences.
- These results motivate comparing the RNN with an LSTM encoder-decoder in Experiment 2.

- I genuinely ran into a lot of errors when using RNNs, not just because of the core bottleneck, but even optimising the dataset up to this point took forever.
  Every training epoch took so long that at least for the time being I've decided to stop trying to optimize, record the observations, and move ahead with LSTMS and
  hopefully get better results (3:30 AM insights lol).

# EXPERIMENT 2 - LSTMS

1. This time, training took a really long time per epoch(4), and the loss was definitely going down as well. But didn't have enough time to train for more epochs.

OBSERVATIONS: 
- LSTM trained for 4 epochs.
- Training loss decreased consistently from 4.1930 → 3.0168.
- Loss reduction was substantial across all four epochs, unlike the RNN, which stabilized around 4.01.
- LSTM training took approximately 862 seconds (~14.4 minutes).
- The LSTM required significantly more training time than the RNN, showing the additional computational cost of maintaining both hidden and cell states.
- The lower training loss suggests that the LSTM is learning the training translation patterns more effectively than the basic RNN.
- Quantitative translation quality still needs to be evaluated using BLEU before concluding generalization.
   
  
  









