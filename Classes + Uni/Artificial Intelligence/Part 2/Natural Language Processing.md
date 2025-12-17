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

#### What some linguist said

"Word contexts are words that appear near them"

the idea is to look at what is going on around me to effectively use the words around me to map myself into this n dimensional space.

Encoder produces window size "called context" even though this is not a contextual encoding

and then it will convert the integer mapping problem into a prediction problem.

$\hat{P}_{data}(W_{t-c},w_{t-c+1},\dots,W_{t+c}|W_{t})$

where the context window are the words given.

the P model uses naive bayes assumption: given the center word, all context words are independent of each other:

so the p model is ![[Screenshot 2025-12-17 at 1.42.32 PM.png]]

then we use the [[Maximum Likelihood Estimation]] 

x is the center word and y is the nearby words

![[Screenshot 2025-12-17 at 1.44.48 PM.png]]

In the past our NN would map a high dim input into a low dim output. 

Now we're gonna have a high dimensional input $\bar{W}_{t}$ which is a |V| dimensional vector. Then a dense matrix W will produce a representation Z, $Z\in \mathbb{R}^{d}$ and this vector should be such that when Z comes in Wj should be a reverde dense NN that will predict $\hat{y}$

So, dimensioning-wise, I need, here at V times D matrix.

I won't…

Simple of use.

Z, J prime, Prime, it doesn't mean I deliver it. Prime here means different.

Because I will form a Z to be WT transpose.

that it will take the Z, and with the help of another matrix.

it will lift it up into the V-dimensional space. So the W matrix is doing the embedding, the projection, and the W prime matrix is doing the lifting.

back to the V-dimensional space.

And,

what else am I missing here? I forgot to mention, you know, here the ZJ prime is my logics, right? Again, 100,000 dimensions, right there, a vector.

![[Screenshot 2025-12-17 at 1.52.06 PM.png]]

![[Screenshot 2025-12-17 at 1.54.50 PM.png | 400]]

We take something in the… 3-dimensional space, Let's say, like a ball. And we will throw it in a lower dimensional space, the floor, In a specific…place in this 3-dimensional kind of floor, right?
That when it is lifting itself kind of up, right? Then we will have, a point in the three-dimensional kind of space that is,That exactly the same,

That exactly the same,

location will be, but from a different point in the three-dimensional space, will be also finding in that similar kind of neighborhood, right? So, similar things are actually happening here. I have…a word in the three-dimensional space, I'm embedding it, projecting it in the Z, in the vector Z, in a D-dimensional space, and this vector is such… this is basically the required embedding. This is basically what I'm after. I'm not after here, I'm not after this guy, I'm not after this guy, I'm just after this, the values of this vector Z. The value of this vector Z will be such that When the specific matrix does the lifting, It will result into a… A vector here whose largest element will actually be a probability that corresponds to the word "drives"

I can take the Z and adjust this WJ minus to produce the nearby war, but at the same time, I have to produce another nearby war that I'm, I'm,I'm seeing frequently in my training data set. And then another and another. There will be four of these branches, in other words, right, in the network. And the training, just like what you have seen, kind of, in the past. 'm seeing frequently in my training data set. And then another and another. There will be four of these branches, in other words, right, in the network. And the training, just like what you have seen, kind of, in the past.

is going to do jointly optimizing the loss. You know, what is the loss? The cross-entropy, in this case, will be some kind of an additive. loss that will sum the loss associated with prediction with J, then the loss associated with prediction with J plus 1, the J-1, and so on and so forth. So all of these kind of mathematics are jointly adapted by the… so basically, essentially the joint optimization that's actually going on to adjust them, such as the Z is such that it is already, as I said, B, in the neighborhood of these other four grains.

Finally we're going to have these two matrices W and W', and then we're gonna freeze W prime and that will be the encoder for our embeddings.

**The problem is that even if we have a word with 2 different meanings (like institution vs river "bank") the place in d-dimensional space where this word is mapped to is somewhere in the middle in between the two.**

**There is only 1 vector for "bank" irrespective of the meaning that may be contained in the input training data.**

