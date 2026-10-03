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

## Attention Results

- Added Luong dot-product attention to the LSTM encoder-decoder.
- Attention model trained for 4 epochs.
- Training loss decreased from 4.1199 to 2.4944.
- Training time was approximately 20.6 minutes.
- Maximum attention training length was limited to 30 tokens to reduce computational cost.
- LSTM + Luong Attention achieved a BLEU score of 0.15913.
- BLEU improved from 0.05326 without attention to 0.15913 with attention.
- Generated translations became more input-dependent and captured more source information.
- <UNK> tokens were still frequent, and some generated phrases remained incorrect or incomplete.
- The results suggest that explicitly attending to encoder outputs improved translation quality compared with relying only on the final encoder state.
- Total training time: 1238.19 sec (~20.6 min).

This is still on training data, right, so test-set BLEU is still needed to determine whether this improvement translated to better generalization.

## Final Conclusion

- The basic RNN encoder-decoder achieved a BLEU score of 0.00286 and showed clear output collapse, producing nearly the same prediction for different inputs.
- Replacing the RNN with an LSTM reduced the final training loss from 4.0123 to 3.0168 and improved BLEU to 0.05326.
- The LSTM also produced more input-dependent translations, although `<UNK>` tokens and incorrect phrases remained common.
- Adding Luong dot-product attention further improved BLEU to 0.15913, compared with 0.05326 for the LSTM without attention.
- Attention allowed the decoder to use information from different encoder hidden states instead of relying only on the final encoder state.
- The attention model produced more source-specific translations and captured more parts of the input meaning, although translations were still imperfect.
- Greedy decoding was used as the main decoding strategy for the reported attention BLEU score.
- Beam search with beam width 3 was additionally tested qualitatively. The generated outputs were not consistently better than greedy decoding across the examples tested.
- Maximum attention training length was limited to 30 tokens for computational efficiency.


Overall, the experiments showed progressive improvement from RNN → LSTM → LSTM with attention, while also demonstrating the computational cost and remaining vocabulary limitations of the baseline system. The final loss was lower than the LSTM baseline's 3.0168, indicating stronger fitting of the training data.

## Model Comparison (Table made using LLLMs)

| Model | BLEU | Final Training Loss |
|---|---:|---:|
| RNN | 0.00286 | 4.0123 |
| LSTM | 0.05326 | 3.0168 |
| LSTM + Luong Attention | 0.15913 | 2.4944 |
| LSTM + Luong Attention + Beam Search | 0.172 | 2.4944 |



  
