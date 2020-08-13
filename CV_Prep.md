# CV_Prep

## Education

### Natural Language Processing

* n-gram models

  * A word in the sentence is decided by its previous n words

* log-linear models

  * Define some features and feature functions

  ![1366616241_3533](CV\1366616241_3533.jpg)

  * Use maximum-likelihood for estimation

* Recurrent Neural Network

  * prior knowledge can handling the sequence

  ![img](CV/1PNG.PNG?lastModify=1597172545?lastModify=1597172545)

  ​

  * RNN may have problem learning long sequence(gradient vanishing problem/gradient exploding problem)

* Long Short Term Memory network

  * special kind of RNN, capable of learning long-term dependencies.

* Encoder-Decoder

  * choose from CNN/RNN/BiRNN/GRU/LSTM, can choose different model in Encoder and decoder.
  * Is encoding the information of the whole sequence into a fix-length vector and then pass into the decoder. The vector may not be large enough for the information or the previous information may be override by the later.
  * Use **Attention** : Decode decides which words to pay attention to at every step. Encoder encodes the sentence into a vector, decoder select part of the vector every step.

* BERT

  * Based on Transformer Encoder

* Beam Search

  * Use bfs to build its searching tree, generate several solution and keep k best solutions and continuing searching. 

* context-free grammar(CFG)

* Bag of words(BOW)

* EM

* ELBO

* Autoencoders (AEs)

* Variational Autoencoders (VAEs)



### Brain Insp Comp

* Traditional neural network is not accurate biologically.
* SNN use spiking to train the network
* Is hard to train as we cannot use  gradient descent



### Massive data mining

* frequent item set

  * Support for itemset I: Number of baskets containing all items in I

  * Confidence of this association rule is the probability of j given I

  * Interest of an association rule I → j: conf(i→j)-pr[j]

  * A-Priori algorithm

    * judging frequent item set

    ![2](CV\2.PNG)

    * PCY Algorithm

* locality sensitive hashing

  * similar items have high possibility of being closed to each other after projection
  * finding similar items

* clustering

  * kmeans
  * BFR 
    * Assume every cluster satisfied normalized distribution
  * CURE
    * Allows clusters to assume any shape
    * Uses a collection of representative points to represent clusters

* dimensionality reduction

  * SVD
  * CUR

* Recommending System