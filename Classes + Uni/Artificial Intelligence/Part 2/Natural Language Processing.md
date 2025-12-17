## [NLP Pipelines](https://pantelis.github.io/aiml-common/lectures/nlp/nlp-introduction/nlp-pipelines/#nlp-pipelines)

## Text Tokenization

Word Level and Subword Level tokenization.

Tokenizer takes text as input, responds with integer numbers. Each number indicates an antry in the vocabulary that we're trying to build. This vocabulary is called V.

### Byte Pair Encoding (BPE)

came from encoding.

EX1:
if we had "aaabdaaabac"

We could replace by saying Z = aa

so "ZabdZabac"

if we then do Y = AB

we get "ZYdZYac"

and we can go on by lowering

EX2: "she sells shells by the shore"

Rank characters by the input frequency:
space: 5
s 5
e 4
h 3
...

then we can do Y = sh

"Ye sells Yells by the Yore"

then our symbols will decrease:

space:5
h: 0 

our vocabulary then decreases.

So when we are going to build we decrease the cardinality (|V|) of V

We pick things to reduce entropy.

as tokenizers get more modern we decrease the length.

tokenizer compression is important because it reduces the context size.

gpt 2 was bad in coding, but modern coding tokenizers replace python words more effectively. 

### Context free embeddings

sequence of tokens -> encoder -> vector $x \in \mathbb{\mathbb{Z}}^{d}$

most trivial way you can do it is a "one code encoding"

so like hotel = \[0,0,0,1,0,0,0,0]
so like motel = \[0,0,0,0,1,0,0,0]

This is a problem because the dot product will not give us any similarity.

We want to have that vectors with similar meaning are near (high dot product)