# Task 2 — Attention Experiment Log

# OBJECTIVE:
- So the main objective of this experiment was to actually apply attention to the already existing RNN/LSTM models.
- Comparing both these models.
- Later compare greedy decoding with beam search.

  # Attention Choice
  - Im using the Luong Attention model, as it was an improvement to the existing Bahdanau attention model that was provided initially.
  - Basically, here we do scaled dot product to get the similarity scores.
  - Then the softmax of these values gives alpha (contextual weights between 2 LSTMs).
 
  # Architecture
- LSTM encoder
- Luong dot-product attention
- LSTM decoder
- Same 10k vocab, embedding size 256, hidden size 512.
- Same dataset and preprocessing as the previous experiment.

# Training

1. So initially using the same model and teacher forcing, adam as optimizer, lr = 0.001 and seq_len = 50 (SAME PARA AS LSTM) i trained the model.
I had to wait for more than 9 mins for just 1 epoch and the Loss that i got was 4.317. When i looked back at LSTM its loss was 4.1930 so obviously something
was wrong as atleast in theory it shldve given better results.

2. Then the first thing that came to my mind when either loss is taking too much time/ overfitting happens is reducing complexity. So here i changes the
max_len to 30, keeping everything else the same ans tried training it.

So why the 30-step change : 

- Attention requires sequential decoder steps because attention is recalculated at every timestep.
- The original 50-token setup was computationally expensive.
- Limited attention training to 30 target steps to make the experiment computationally manageable.
- Original RNN/LSTM baselines remain unchanged.

# Training Data

- Luong attention model trained for 4 epochs.
- Training loss decreased consistently: 4.1199 → 3.1436 → 2.7383 → 2.4944.
- Total training time: 1238.19 sec (~20.6 min).
- Attention training was substantially slower because attention was recalculated at each decoder timestep.
- Maximum attention training length was limited to 30 tokens for computational efficiency.
- Final loss was lower than the LSTM baseline's 3.0168, indicating stronger fitting of the training data.
- Test-set BLEU is needed to determine whether this improvement translated to better generalization.

2
