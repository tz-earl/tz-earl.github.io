---
layout: post
title:  "Using a Recurrent Neural Network to Generate 'Old English' Names"
date:   2020-02-16 00:00:00 -0700
categories: 
---
While learning about sequence models as part of the [Deep Learning set of courses offered by deeplearning.ai](https://www.deeplearning.ai/deep-learning-specialization/) one of the exercises was to implement bits and chunks of code of a recurrent neural network that was trained on a set of known names of dinosaurs and then used to generate novel dinosaur-sounding names that don't exist.

The following diagram gives an overview of how this RNN works when generating names. There is actually just one cell that is repeatedly activated by a sequence of inputs, where time is moving from left to right. Each input **_x_** consists of a single English letter or a newline character (which is a separator for the names). Each input **_a_** represents internal state that depends on the part of the sequence previously seen.  Each output **_ŷ_** is one letter chosen based on a probability distribution that was created by the training phase. The accumulation of these outputs forms a generated name.

<image src="{{site.url}}/images/rnn-to-generate-names.png" alt="Recurrent neural network to generate names" />
<br />

I thought it would be a fun exercise to apply this RNN model to generate a different kind of name, namely, Old English-sounding people names.

I found a website with [a set of 299 Old English girls names]({{site.url}}/files/oe-girl-names.txt) and [a set of 300 Old English boys names]({{site.url}}/files/oe-boy-names.txt) that serve as training data. I used the girls and the boys names as separate data but also concatenated them into a combined data set.

Using the boys names, 40_000 training iterations, and default values for the other parameters resulted in the following output, with selected snapshots along the way.

~~~~~
Iteration: 2000, Loss: 21.291615

Mhxvtrbn
In
Iyrtrbmdlldwerlrareletr
Md
Ystrbmdlldwerlrareletr
D
Uuran

Iteration: 18000, Loss: 13.390953

Pewton
Melbard
Murpt
Rad
Wirk
Hacford
Tore

Iteration: 34000, Loss: 12.374373

Rawston
Racfartreabroough
Ruwson
Racearumae
Worton
Macenteldrocm
Tronrerley

Iteration: 38000, Loss: 11.951417

Raynord
Radbarrorallon
Ritton
Rad
Uxrield
Rackley
Stanfield

Iteration: 40000, Loss: 11.763989

Raynnelbridft
Redcalllid
Rutton
Rach
Trowdeld
Hachet
Rouke
~~~~~

The values of the loss function quickly flatten out, generating some names that sound pretty good! I especially like **Rawston**, **Raynord**, and **Worton**. The model also generated **Stanfield**, which is an existing name.

Using the girls names and the combined set yielded similar results. The first iterations more or less produce garbage, then the loss function drops fast and plausible names start to come out.

Some other names that came out of the combined set were **Reynora**, **Wyrth**, and **Lytira**.

<hr width="80%">
<br />
I decided to try increasing the number of training iterations and the learning rate.

Increasing the iterations from 40_000 to 50_000 produced:

~~~~~
Iteration: 48000, Loss: 10.967822

Raynon
Radcad
Rutster
Rachaston
Wyndan
Ladcnesbarn
Tondell


Iteration: 50000, Loss: 10.976192

Raylot
Rack
Rowlon
Rachat
Worton
Olally
Wilks
~~~~~

To 60_000:

~~~~~
Iteration: 58000, Loss: 11.220603

Raynley
Leich
Lowst
Rafbert
Worthall
Eabe
Word


Iteration: 60000, Loss: 10.235180

Raynop
Leich
Lowsten
Rachash
Wostar
Eabert
Tondolp
~~~~~

Finally, to 70_000 iterations, at which point most of the generated names looked very plausible.

~~~~~
Iteration: 68000, Loss: 10.159134

Enwrid
Esled
Eswood
Ellad
Wyntel
Edbert
Remar


Iteration: 70000, Loss: 10.670650

Remley
Lefabe
Lowler
Radallege
Worthwele
Edbert
Won
~~~~~

<hr width="80%">
<br />
I also tried decreasing the learning rate from the default value of 0.01 to 0.005.

<hr width="80%">
<br />
**Later:** while in the midst of writing this blog post, I happened to discover the [blog post by Andrej Karpathy](https://karpathy.github.io/2015/05/21/rnn-effectiveness/) on recurrent neural networks in which he briefly describes using an RNN to generate general baby names. His post was written in 2015 so his work predated mine by several years, 8-D

<hr width="80%">

<br />
**Sources:**

&nbsp;&nbsp;&nbsp;&nbsp;_Sequence Models_  
&nbsp;&nbsp;&nbsp;&nbsp;<https://www.coursera.org/learn/nlp-sequence-models>

&nbsp;&nbsp;&nbsp;&nbsp;_Old English Girl Names_  
&nbsp;&nbsp;&nbsp;&nbsp;<http://www.top-100-baby-names-search.com/old-english-girl-names.html>

&nbsp;&nbsp;&nbsp;&nbsp;_Old English Boy Names_  
&nbsp;&nbsp;&nbsp;&nbsp;<http://www.top-100-baby-names-search.com/old-english-boy-names.html>

&nbsp;&nbsp;&nbsp;&nbsp;_The Unreasonable Effectiveness of Recurrent Neural Networks_  
&nbsp;&nbsp;&nbsp;&nbsp;<https://karpathy.github.io/2015/05/21/rnn-effectiveness/>

