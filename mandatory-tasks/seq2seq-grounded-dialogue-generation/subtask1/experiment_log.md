# Subtask_1 — Experiment Log

NOTE: My entire experiment and a lot of findings/readings Ive taken is based on the research paper
that i read - Seq2Seq Learning with nn (-Sutskever et al. 2014).

## GOAL

- Our baseline goal is to establish a simple baseline model for English-to-French translation.
- Implement an encoder-decoder structure from scratch without using any form of attention.

---

## Dataset/ Preprocessing

English → French parallel corpus.
232,825 training pairs, 890 validation, 8,597 test.
10,000-word vocabulary for each language.
<PAD>, <SOS>, <EOS>, <UNK> special tokens.
Maximum sequence length = 50.
Batch size = 64.

## Architecture

RNN encoder.
RNN decoder.
Embedding dimension = 256.
Hidden dimension = 512.
Encoder compresses the source sentence into its final hidden state.
Decoder uses that hidden state to generate the French sentence token by token.
No attention.

## Model Size

Encoder: 2,954,240 parameters
Decoder: 8,084,240 parameters
Total: 11,038,480 parameters

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

How well a basic fixed-context encoder-decoder translates.
Whether it struggles with longer sentences.
Whether rare/unknown words cause problems.









