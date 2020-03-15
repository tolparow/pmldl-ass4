# Assignment 4
## PMLDL

#### Ilya Dubovitsky, Nikolay Matyashov, Taimuraz Tolparov

### Introduction

We approached the solution of this task using three different models, including Attention, Transformer and custom Encoder-Decoder (LSTMs based).

Shortly, the best performing model is Transformer, producing the best result in a limited amount of training time. After comes Attention, which has comparable performanse, although it reqires much longer time to achieve same accuracy. The last one is our own trial to create an Encoder-Decoder LSTM model, that is based on lecure materials, which is due to the lack of time and experience performs worst, trains much longer then others.

### Models description 
#### Encoder-Decoder with LSTM

![](https://i.imgur.com/J6D9QHw.png "Encoder-Decoder (LSTM)")

As Encoder-Decoder model based on LSTMs was mentioned in the lecture, we decided to implement it by ourselves and compare results with state-of-the-art models. 
So, as it can be seen from it can be seen from the image, Encoder is a LSTM (lstm_3), which processes preprocessed input data (data formating + embeddings creation) one element at a time for each sentence, then propagates information collected further to decoder. Decoder is a second LSTM (lstm_4), that gets hidden states from encoder and true translation from input data when training so that it produces predictions finally. Also, to actual translation _START and END_ tokens are added to correctly initialize initial and final states of LSTM, just to understand start and end of the sentence.
The complete flow is: Input sentence proposed to Encoder word by word -> final internal state is produced by Encoder, which containes info about sentence -> translation and internal state forced to decoder -> decoder predicts an output at each time step.

#### Encoder-Decoder with GRU and Attention

This model has similar architecture with the previous one but additionally uses attention layer for better decoding on long sentences. Also it has GRU layers instead of LSTM, because it allows to train the network faster.

#### Transformer 

And the last model we used is the Transformer model. For the encoder it uses Embedding layer followed by Positional encoding to add information about word position in sentence. Then it  uses multi-head attention that generates several self-attention matrices and averages it for each word. Then it processed through feed forward layer. Decoder has similar architecture. The difference is the second multi-head attention layer that take encoder’s output as additional input. 

### Modeling Process
#### LSTM based Encoder-Decoder



#### Datasets

We used several datasets, including UN (~23M), Yandex (~1M), Paracrawl (~1M), Wikipedia Names, manythings (300k).

The best quality of datasets was present in Yandex and manythings, thus we composed our training datasets from them mostly (up to 80%). Paracrawl has bad quality, UN has complicated and unusual words, has some unpredictable translations which are misleading (e.g. addition of word "Official" to some strings in Russian translation).

Datasets are located at [Google Bucket](https://console.cloud.google.com/storage/browser/pmldl-rus-en).

### Results

| Model | BLEU | Epochs | Dataset size |
| -------- | -------- | -------- | -------- |
| Encoder-Decoder LSTM | 0.64 | 180 | 300k |
| GRU + Attention | 0.92 | 21 | 95k |
| Transformer | 9.11 | 300 | 300k |
| Transformer | 40.83 | 300 + 200* | 300k + 1M |

\* -- Transformer was additionaly trained on Yandex dataset for 200 epochs, after training on a base small dataset of 300k elements composed from several different datasets.

### Code

We used Google Cloud AI Platform Jupyter Lab with free credits for overnight training, Google Colab for collaborative work. 

Code available: [Github repo](https://github.com/tolparow/pmldl-ass4).
