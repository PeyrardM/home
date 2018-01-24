---
layout:     post
title:      "Entropy and Complexity [IT.2]"
subtitle:   "Introduction to algorithmic information theory"
date:       2018-01-14 12:00:00
author:     "peyrardm"
header-img: "img/bg_home.jpg"
---

<p>In a prior article, we introduced Shannon's entropy and the resulting interpretations of information.  Now, we describe an alternative view on information. While Shannon {% cite Shannon_1963 -A %} approached it from a probabilistic perspective, Kolmogorov {% cite Kolmogorov65 -A %}, Solomonoff {% cite SOLOMONOFF1964 -A %}, and Chaitin {% cite Chaitin1989 -A %} related information to computation in a profound way using Turing machines. They formed the field of <em>Algorithmic Information Theory</em>, which proposes the <em>Kolmogorov Complexity</em> as the main quantity.</p>

<!-- [https://homepages.cwi.nl/~paulv/papers/info.pdf]. -->

<h2>Limitation of Shannon's Information</h2>
<hr>
<p>Consider the question of estimating the amount of information in a given text. </p>

<p>Information theory assumes that objects are outcomes of a random source. In this case, information is quantified by entropy. In fact, Shannon's notion of information ignores properties of the objects and only the characteristics of the random source determines the information theoretic properties.</p>

<p>Hence, before measuring the entropy of a specific text, we must define a general probabilistic model: <br>
- We may model text generation as a stochastic process with each word being drawn from some distribution. However, a realistic model should account for syntax, semantic, the author's mind and any other contextual variables that influenced the writing process. <br>
- Otherwise, we can view the whole text itself as an element drawn from the set of all possible texts. This also appears difficult since we must specify a meaningful probability distribution on this huge set. 
</p>

<p>Alternatively, we can search for a measure of information that does not rely on (unrealistic) probabilistic assumptions. This measure would be independent of the knowledge of an observer and the information content of an object \(x\) would be an intrinsic property of \(x\).</p>

<p>The <b>Kolmogorov complexity</b> is a measure satisfying these requirements. It views complexity as a fundamental property of an object based on regularity, symmetry, and compressibility. It is tied to computation because the complexity of an object is the length of the shortest programme producing it. Kolmogorov complexity does not need the assumption about the message coming from a random source, it measures the minimum number of bits from which a specific message can be effectively reconstructed.</p>

<!-- <p>Shannon's entropy measures the minimum expected number of bits to transmit a message from a random source with known characteristics. Kolomogorov complexity does not need the assumption about the message coming from a random source, it measures the minimum number of bits from which a specific message can be effectively reconstructed. </p> -->

<!-- <p>By analogy with the previous post, imagine a system \(X\) that can be in \(N\) different states. Our knowledge about the behavior of \(X\) is encoded into a probability distribution. Shannon's entropy measures our uncertainty about \(X\) based on the assumption that \(X\) is a random source with some distribution. Kolmogorov complexity aims at measuring the intrinsic information of specific states \(w_i\). It is indepedent of the existence or behavior of \(X\) and therefore indepedent from our knowledge about \(X\). Instead it is a property of \(x_i\) alone</p> -->

<!-- <p>Kolmogorov complexity is not dependent on the knowledge of the observer and is a fundamental property of object but this renders its computation and estimation rather difficult in practice. In this post, we'll discuss Kolmogorov complexity and how it connects to Shannon's entropy. We'll also see the connexion with Gödel's incompleteness theorem.</p> -->

<h2>Kolmogorov Complexity</h2>
<h3>Turing Maching</h3>
<p>A Turing machine (TM) consists of 3 tapes: the program tape, the work tape and the output tape.  For simplicity and without loss of generality, we can focus on binary alphabets \(\{0,1\}\). 
The program tape is "read-only" and can just move to the right.
The output tape is "write-only" and also move only to the right.
The working tape is 'read and write' and can move in both directions.
A Turing machine has a finite number of internal states and its behavior is determined by a function, which maps the current state and the content of the currently read square on the working tape to a new state and an action.</p>

<p>A finite binary string \(p\) written on the program tape is a <b>program</b> if and only if a Turing machine M reads all bits of \(p\) and halts. This implies that no program can be the prefix of another one. </p>

<p>It is well-known that there exists a universal Turing Machine \(U\): For any Turing Machine \(C\) there exists a constant prefix \(\mu_C\) such that:
$$
\forall p \in P, C(p) = U(\mu_Cp).
$$
\(P\) is the set of programs and the prefix \(\mu_C\) is the compiler that compiles programs written for \(C\) into equivalent programs for \(U\).</p>

<p>Since Turing's work, we acknowledge that any computable mathematical object can be described by a binary string. Consequently, an object \(x\) can be identified by a binary string. Intuitively, the complexity of \(x\) (or amount of information) is related to the structure of the binary string representation. We say that \(x\) is complex if it can't be compressed much.</p>

<h3>Complexity</h3>
<p>We call a description of \(x\) a program \(s_x\) that generates \(x\): \(U(s_x) = x\).
An object \(x\) might have several descriptions but each description specifies only one object.</p> 

<p>The Kolmogorov complexity of an object \(x\) is defined as the length of its shortest description:
$$
K(x) = \min\limits_{p} \{l(p) | U(p) = x\}
$$ 
Intuitively, an object is simple if it can be described briefly and <em>complex</em> if it requires a long description. The intuition is similar in communication theory, where descriptions are replaced by messages. </p>

<h3>Conditional Complexity</h3>
<p>Suppose we have a program \(p\) running on the universal Turing machine \(U\). Until now, we considered programs without input data, they simply run and output \(x\). </p>

<p>Now, we note \(<p,y>\) the program taking as input the string \(y\). We call  <b>conditional complexity</b> of \(x|y\) the size of the smallest program generating \(x\) given that \(y\) is used as input. 
$$
    K(x|y) = \min\limits_{p} \{l(p) | U(<p,y>) = x\}
$$
Therefore, we have \(K(x) = K(x | \epsilon)\) where \(\epsilon\) is the empty input. Since \(y\) can bring some information about \(x\) it might be possible to devise a shorter program to generate \(x\). In the limit, \(y=x\) and there can be an extremely simple program that just copies its entry \(y\) to the output. Conversly, if \(y\) does not contain any information about \(x\), then \(K(x|y) = K(x)\) and this is the equivalent of independence in probabilistic terms.</p>

<h3>Consequences</h3>
<p>When \(K(x) \approx l(x)\),  \(x\) is incompressible, it has no structure and therefore random. Finding a shorter description is mapping from strings to strings and there are many more long strings than short ones. Most strings cannot possibly have a short description and therefore most strings are random.</p>

<p>It is important because it reveals that <em>simplicity</em> and structure are special. Most computable objects are random. Generalization and learning can be thought of instances of simplification or finding structure and regularity: we can only generalize if there is some underlying structure. Most objects are not learnable which is surprising because we see structure everywhere in the world.</p>

<p>Unfortunately, \(K(x)\) is not a recursive function: we generally cannot compute it from the complexity of substrings of \(x\). \(K\) is not computable in general, but there exists useful extension of \(K\) like Levin Complexity (To be discussed in later posts).</p>

<!-- Suppose we could find \(z\) t -->

<h2>Complexity and Entropy</h2>
<p>Suppose we have a random variable \(X\) taking the state \(x_i\) with a probability distribution \(P(X=x_i) = p_i\). 

In Shannon's view, the entropy of \(X\) is: \(H(X) = -\sum\limits_{x_i} p_i \cdot \log(p_i)\), where \((-\log(p_i))\) is the surprise of observing the state \(x_i\).
From Kolmogorov's perspective, \(K(x_i)\) is fixed for each \(x_i\) and independent of \(P\).</p>


<p>The surprise of observing \(x_i\) is the Shannon equivalent of the intrinsic information of Kolmogorov. Therefore, we may wonder whether \(\sum\limits_{x} p_i \cdot K(x_i)\) ressembles \(H(X)\). Indeed, \(K(x_i)\) is the shortest description of \(x_i\), meaning we could encode \(x_i\) optimally. With Shannon, we have optimality on average. In fact, there exists a theorem saying that:
$$
	0 \leq (\sum\limits_{x} p_i \cdot K(x_i) - H(X)) \leq K(P) + O(1)
$$ 
This means that for simple distributions on states the expected Kolmogorov complexity is close to Shannon's entropy. These two quantities might be wide apart for complex distribution however. </p>

<h2>Complexity and Information</h2>

<p>Shannon's entropy can be understood as potential information, a measure of how much more we can learn about the system in order to predict perfectly its behavior. In Kolmogorov theory, low complexity is revealed by regularity and symmetry. Complexity originates in symmetry breaking.</p>

<p>When we compared a text \(Y\) made of one repeated word and \(Z\) outputting proper English, we noticed that \(Z\) is more interesting because it has more potential information. However, we would not find a string of purely random characters particularly interesting. It may be because random means high complexity, lack of symmetry or absence of simple rules behind the object. We cannot expect to discover elegant simple laws behind a random text. </p>

<p>Maybe, something is subjectively interesting if it has high potential information but low complexity. Indeed, if the object of study has high potential (based on our current knowledge), we could still learn a lot about it. However, if it has a high complexity, the object is basically random and we can never find simple laws to explain it. Finally, low complexity means <em>learnable</em> and high entropy means <em>yet to be learned</em>.</p>

<p>The scientific questions we find fascinating like the brain, consciousness, intelligence or the laws of the universe are exciting because we expect them to be learnable (low objective complexity: not random and governed by simple symmetric regular principles) while yet to be learned (high subjective entropy: currently high uncertainty about them). </p>

<p>Shannon's entropy is about measuring information in term of what we know encoded in conditional probability distributions. It supports a Bayesian view of the world and knowledge acquisition. Kolmogorov's complexity is independent of presupposition and captures an intrinsic property of an object by quantifying the amount of structure and regularity. Surprisingly, the two different perspectives turn out to be connected via the notion of compressibility: bringing together the notions of information via knowledge and information via structure/pattern. </p>

<p>This connection is rather intuitive as we already expected that a system with regular patterns can be compressed into a generalizable model. Both perspectives account for this view of information by compression and generalizability. </p>


<!-- <h2>Conclusion</h2>
<p> Shannon's entropy is about measuring information in term of what we know about a system encoded in conditional probability distributions. It supports a bayesian view of the world and knowledge acquisition.
Kolmogorov's complexity is independent of presupposition and it is an intrinsic property of object quantifying the amount of structure and regularity. Surprinsigly, The two different perspectives turn out to be connected and close to each other <em>in the limit</em> bringing together the notions of information via knowledge and information via structure/pattern. This connection is rather intuitive as we already expected that a system with regular patterns can be compressed into a generalizable model. Both theories account for this view of information by compression. 
 However, they both come with difficutlties: Shannon's view requires assumptions about the systems, its states and its probability distribution while Kolmogorov's complexity is neither computable nor easily estimable.
Nevertheless, These frameworks are relevant because they allow rigorous discussion of knowledge acquisition, learning, pattern and structure emergence. All of which are profound and challenging questions of modern science.</p> -->

# References
{% bibliography --cited %}