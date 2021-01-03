---
title: Concepts In Natural Language Processing [more to come]
permalink: concepts_in_natural_language_processing.html
date: 2020-01-28
tags: [deep_learning]
published: false
---

The field of **natural language processing (NLP)** roughly falls into two interrelated research aspects, **the natural
language understanding (NLU)** and the **natural language generation (NLG)**. It is a wise way to embark on our
discussion with concepts in natural language processing.

### Tokenization (Word Segmentation)
Although there exists little consensus for the definition of tokens, however, in most of typical cases, the concept of
token simply correspond to words. Thus, tokenization is more difficult for Chinese than for English, since the latter
has space or tabular characters as its default word segmentation symbols. In addition, the output of Chinese
tokenization algorithms might correspond to different levels of granularity, which should be decided according to
specific use cases. While the English tokenization algorithms are relatively simpler, as opposed to Chinese, they
require additional processing known as lemmatization and stemming. For Chinese tokenization, we use dictionary based
algorithms, statistical algorithms (such as hidden Markov models (HMM) and conditional random fields (CRF)), or hybrid
algorithms of both.

The difficulty of Chinese word segmentation arises in part from ambiguities associated with it. Let us investigate a
couple of real-world examples, whose precise tokenization should rely on sufficient contextual information.

Example 1:  
more preferable: 结婚/的/和/尚未/结婚/的 (those married and those not yet married)  
less preferable: 结婚/的/和尚/未/结婚/的 (married monks and the unmarried)  

Example 2:  
more preferable: 南京市/长江/大桥 (Nanjing Yangtze River Bridge)  
less preferable: 南京/市长/江大桥 (Nanjing Mayor, Daqiao Jiang)  

Example 3:  
more preferable: 研究/生命/的/起源 (do research on the origin of life)  
less preferable: 研究生/命/的/起源 (the origin of postgraduates' life)  

### Part-Of-Speech (POS) Tagging
POS tagging associates each word with its corresponding word class, such as noun, pronoun, verb, etc. In most natural
language processing packages, word class is further subdivided into finer categories, such as ns for location name, nr
for person name, nrf for foreign person name. Some Chinese word segmentation packages (or Chinese natural language
processing packages which provides word segmentation functions) simultaneously solves for word segmentation and POS
tagging using HMM, which implicitly assigns the Chinese characters inside each segmented word self-explanatory internal
labels (using Viterbi algorithm) including B (begin), M (middle), E (end) and S (single).

<img src="{{ "images/20200128-1.png" }}" alt="postag"/>
_Figure 1: A part-of-speech tagging example._

### Chunking
The chunking is a higher-level technique, which segments and labels multi-token sequences on top of word-level
tokenization and part-of-speech tagging. Like tokenization, which omits whitespace, chunking usually selects a subset of
the tokens. Also like tokenization, the pieces produced by a chunker do not overlap in the source text.

### Chunk Tagging Schemes
The chunking task requires chunk tagging schemes using tags, which consist of tag B for the beginning token in the
chunk, tag I for intermediate token(s) in the chunk, tag E for the ending token in the chunk, tag S for the token in the
single-token chunk, and tag O for other tokens uncorrelated with a chunk, such as punctuations. Based on part or all of
these tags, widespread tagging schemes, as adopted by the research community and the industry as well, include but are
not limited to the following format variants:

IOB1: tag I is applied to intermediate token(s) inside the chunk, tag O is applied to token(s) outside the chunk, and
tag B is applied to the beginning token of the chunk only if the chunk is preceded by a chunk of the same type.

IOB2: it differs from IOB1 only in that tag B is applied to the beginning token of every chunk other than type S or O.

IOE1: tag I is applied to intermediate token(s) inside the chunk, tag O is applied to token(s) outside the chunk, and
tag E is applied to the ending token of the chunk only if the chunk is succeeded by a chunk of the same type.

IOE2: it differs from IOE1 only in that tag E is applied to the ending token of every chunk other than type S or O.

START/END: (also called SBEIO, IOBES): all 5 tags are applied, where tag S is applied to the token in single-token
chunks, tags B, I and E are always applied to the beginning, the intermediate and the ending token(s) of the chunk
respectively in multi-token chunks, and tag O is applied to other tokens uncorrelated with chunks.

IO: only tags I and O are applied. Nevertheless, this simple scheme is unable to discern consecutive chunks of the same
type.

### Case Study: A Chunk Tagging Example
Let us introduce a chunk tagging example by the following sentence:

He reckons the current account deficit will narrow to only # 1.8 billion in September.

The sentence is divided as follows:

[NP He ] [VP reckons ] [NP the current account deficit ] [VP will narrow ] [PP to ] [NP only # 1.8 billion ]
[PP in ] [NP September ] .

## Named Entity Recognition (NER)
Named entities (NEs) are definite noun phrases that refer to specific types of individuals, such as organizations,
persons, dates, and so on. Named entity recognition (NER) refers to a subtask of information extraction that seeks to
locate and classify named entities in text into pre-defined categories such as the names of persons, organizations,
locations, expressions of times, quantities, monetary values, percentages, etc. Named entity recognition uses rule-based
algorithms, statistical algorithms (such as HMM, CRF or maximum entropy models), or hybrid algorithms of both. Named
entity recognition is closely related to noun phrase chunking (NP-chunking), a subtask that plays an important role in
the task of chunking.

### Case Study: Text Features for Named Entity Recognition
The natural language processing tasks require uniform text features shared among multiple source texts. One typical
feature set for named entity recognition used in CoNLL2002 corpus is available in the nltk package, like this:

(<I>token</I>, <I>POStag</I>, <I>label</I>)  
---------------------------------------------  
[('Melbourne', 'NP', 'B-LOC'),  
('(', 'Fpa', 'O'),  
('Australia', 'NP', 'B-LOC'),  
(')', 'Fpt', 'O'),  
(',', 'Fc', 'O'),  
('25', 'Z', 'O'),  
('may', 'NC', 'O'),  
('(', 'Fpa', 'O'),  
('EFE', 'NC', 'B-ORG'),  
(')', 'Fpt', 'O'),  
('.', 'Fp', 'O')]

which featurizes the source text sentence "Melbourne (Australia), 25 may (EFE)." into the required format. In this
example we use word identity, word POS tag and word label as the only features associated with each word. Also, some
contextual information can be incorporated into it. The last column represents the true answer label (chunk tag) which
is going to be learned and predicted by language models such as CRF.

## Keyword Extraction
more to come...

## References
Steven Bird, Ewan Klein, and Edward Loper. 2009. Natural Language Processing with Python. Chapter 7.

{% include links.html %}
