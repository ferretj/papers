# [Reading Wikipedia to Answer Open-Domain Questions](https://arxiv.org/pdf/1704.00051.pdf) 

by: **Danqi Chen, Adam Fisch, Jason Weston, Antoine Bordes (Stanford, Facebook AI Research)**

## tl;dr
Use Wikipedia as only source of knowledge to answer open-domain questions (by citing a Wikipedia text span). This is done in 2 parts : 
document retrieval : finding related documents   ->  by bigram hashing and TFIDF matching
machine comprehension of text : identifying text spans as answers   -> by multi-layered RNN

## Notes

factoid questions = questions whose answer provides a concise fact
open-domain questions = questions whose answer is not a list of relevant documents but a short text in natural language

Knowledge Bases (KBs) such as DBpedia are easily processed but too sparse for open-domain QA ≠ Wikipedia contains up-to-date info but was designed for humans to read

goal = find relevant docs among 5 million and extract answers -> machine reading at scale (MRS)
does not use graph structure of Wikipedia

whole model = Document Retriever + Document Reader

Document Retriever outperforms built-in Wikipedia search engine

Document Reader is state-of-the-art on SQuAD

multitask learning helps get better performance on all datasets tested

different from other approaches that use KBs and heteregeneous content additionally
different from approaches in which a small passage containing the answer is given as input to the model

**Document Retriever**

non ML document retrieval system
articles and questions are compared TFIDF weighted bag of words constructed by inverted index lookup
improvement using local word order and bigrams
bigrams are mapped to 2^24 bins using the hashing trick (see Feature hashing for large scale multitask learning by Weinberger et al) with murmur3 hash

-> set to return 5 Wikipedia articles

**Document Reader**

*1. Paragraph Encoding*

treat paragraphs as a sequence of tokens and feed token features to RNN :

* word embeddings
* exact matching (3 feat.) : 1 if token in question (original form, lowercase, lemmatized)
* token features : NER, POS, normalized term frequency
* aligned question embedding : weighted sum of the question word embeddings

weights = attention score between question token and paragraph token

attention is modeled as dot product between non-linear mappings of word emb

![](/imgs/drqa.png)
                                                      
where E(.) is word embedding operator and ɑ is single dense layer + ReLU (see Learning recurrent span representations for extractive question answering by Lee et al 2016)

interesting ideas :

* use pretrained embeddings (GloVe, 300 dim)
* only fine-tune 1000 most frequent words because their representation (f.i. what, where...) could be crucial for QA
* aligned question embedding link similar non-identical words (f.i. car and vehicle)

*2. Question Encoding*

concatenation of all hidden states of a recurrent network on question word embeddings
aggregation is a weighted sum 

![](/imgs/drqa2.png)

where q is a vector to learn

*3. Prediction*

aim is to predict token span most likely to contain answer in paragraph
2 classifiers : 1 for beginning, 1 for end

a bilinear term is used

![](/imgs/drqa3.png)

best span is chosen as argmax of Pstart(i) * Pend(i’) where i <= i’ <= i + 15
score across paragraphs is computed as softmax of unnormalized Pstart(i) * Pend(i’), with all the candidate paragraphs (even across several documents) are used

**Experiments**
 
features computed using coreNLP
three layers of bidirectional LSTM used for both question and paragraph
dropout = 0.3 everywhere

**Limitations**

not end-to-end : two systems trained separately (Reader & Retriever) 
