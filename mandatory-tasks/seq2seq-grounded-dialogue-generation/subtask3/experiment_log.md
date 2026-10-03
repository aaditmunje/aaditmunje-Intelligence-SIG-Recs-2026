# SUBTASK 3 

# My documentation errors encountered (Till Tokenization)

1) For this specific task i wanted to be able to implement all the given tasks properly and not spend 2 hours just training like last task.
So my main goal was to a) demonstrate what kinda architecture im using, b) Documentation Grounding.

2) Previously, when i worked on Transformers (in my IEEE project) it was a simple CodeT5 transformer that we fine-tuned and had just 1 encoder per
decoder. Here it took me a while to understand that we would actually need 2 encoders. The model shldnt just respond on what the previous user said but
should also be able to state facts from the document when generating reply.

3) When I started doing preprocessing, the first thing i noticed was that the dataset was giving me WikiDocIdx = 14 and docIdx=0, but not the actual WikiText
in that particular row. So we needed to find eg for WikiIdx 14 -> Wiki article abt that movie -> Section 0 of article -> actual text.

So basically found out that we cldnt use WikiDocIdx=14 directly, as 14 is NOT the doc itself, all its saying is go look up doc 14, docIdx=14 and use section 0
of that doc.

3) Further, when i printed WikiData out it was evidend that it isnt just 1 big string. It's structured into fields like cast,plot etc. So we had to change our
preprocessing tactic slightly. Instead of processing part by part we're combining useful fields into 1 doc text (each key has a diff component of text).

4) While building the Doc ka dictionary for individual words i found out that atleast 1 of the 4 sections have a string component so it was giving error.
Tbh i didnt know how to debug it so when i prompted an LLM it told me to take both sections in 2 cases ie

- if section is dictionary : Take out all its fields.
- if section is alr text : Use the text.

This entire task took me quite long as the text itself was very complex. Will move ahead with making architecture and trainig. Below is some data that we got
from the dataset: 

## Data Loading and Preprocessing

- Loaded the CMU Hinglish DoG dataset from Hugging Face.
- Dataset splits: 8060 train, 942 validation, 960 test.
- Retrieved the corresponding WikiData files from the original CMU DoG repository.
- Built a mapping from `wikiDocumentIdx` to the corresponding Wikipedia document.
- Each Wikipedia document contains four sections with structured information such as movie name, introduction, cast, director, genre, rating, year, and critical response.
- Combined the four sections into a single text representation for each grounding document.
- Constructed training examples using the previous 3 dialogue turns as conversation history, the corresponding grounding document, and the next Hinglish turn as the target response.
- Limited the grounding document to the first 120 words to keep the model input manageable.
- Used custom Hinglish tokenization with lowercase conversion and punctuation separation.
- Built the vocabulary using training data only, with special tokens `<PAD>`, `<SOS>`, `<EOS>`, and `<UNK>`.
- Vocabulary size was limited to 8000 tokens.
- Set maximum lengths of 30 tokens for dialogue history, 80 tokens for documents, and 30 tokens for target responses.
