---
layout: post
title:  "Using a Recurrent Neural Network to Generate 'Old English' Names"
date:   2020-02-15 00:00:00 -0700
categories: 
---
While learning about sequence models as part of the [Deep Learning set of courses offered by deeplearning.ai](https://www.deeplearning.ai/deep-learning-specialization/) one of the exercises was to implement bits and chunks of code of a recurrent neural network that was trained on a set of known names of dinosaurs and then used to generate novel dinosaur-sounding names that don't actually exist.

The following diagram gives an overview of how this RNN works when generating names. There is actually just one cell that is repeatedly activated by sequences of inputs, where time is moving from left to right. Each input x consists of a single English letter or a newline character (which is a separator for the names). Each output ŷ is one letter chosen based on a probability distribution that was created by the training phase. The accumulation of these outputs forms a generated name.

<image src="{{site.url}}/images/rnn-to-generate-names.png" alt="Recurrent neural network to generate names" />
<br />

I thought it would be a fun exercise to apply this RNN model to generate a different kind of name, namely, Old English-sounding people names.

I found a website with [a set of 299 Old English girls names]({{site.url}}/files/oe-girl-names.txt) and [a set of 300 Old English boys names]({{site.url}}/files/oe-boy-names.txt) that serve as training data. I used the girls and the boys names as separate data but also concatenated them into a combined data set.

Using the boys names, 40,000 training iterations, and default values for the other parameters resulted in the following output, with selected snapshots along the way.

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


<br />
**Sources:**

&nbsp;&nbsp;&nbsp;&nbsp;_Sequence Models_  
&nbsp;&nbsp;&nbsp;&nbsp;<https://www.coursera.org/learn/nlp-sequence-models>

&nbsp;&nbsp;&nbsp;&nbsp;_Old English Female Names_  
&nbsp;&nbsp;&nbsp;&nbsp;<http://www.top-100-baby-names-search.com/old-english-female-names.html>

&nbsp;&nbsp;&nbsp;&nbsp;_Old English Names for Baby Boys_  
&nbsp;&nbsp;&nbsp;&nbsp;<http://www.top-100-baby-names-search.com/old-english-names-for-baby-boys.html>

