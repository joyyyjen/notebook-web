+++ 
title = "Error Repetition Hyphen" 
date = 2019-02-13T15:48:54-06:00 
tags = []
featured_image = "" 
description = "" 
+++

### Training Error Identified
When I was training Astrom Audio, the following word are not in dictionary:

> * 'week-long' 0
> * 'an-and' 2
> * 'multi-disciplinary' 3
> * 'valkenburg' 4
> * 'ehht' 5
> * 'creatives'' 6
> * 'cross-fertilization' 7
> * 'far-reaching' 9
> * 'it's-it's' 10
> * '1964' 13
> * 'the-the' 14
> * '5th' 15
> * '15th' 16

72% of the sentences fail to produce phonetic transcription because of a single word missing in the dictionary model.

**How can we fix this issue?** 

### Repetition
* 'an-and'
* 'it's-it's'
* 'the-the'

In the speech to text process, do we want to output the colloquial duplication or the unintended repeat of the utterance? 

### Number
In our phonetic dictionary, we only have numbers in word form. It conflicts with our reference text.
Should we delete it from the error list? Or can we add the number into the phonetics dictionary?

### Hyphen (-)
The hyphen is used in compound words or to join prefixes to other words. 
For instance, "multi-disciplinary", "well-being" ... <br>
In CMU English phonetic dictionary, some words like ad-hoc, air-force have a hyphen, while words like "weeklong", "multination" do not have a hyphen.
Why is it the case?
One possible solution is to use collocation to update the dictionary with appropriate compound words.

Reference: 
[Errors, Repetition, and Contrastive Emphasis in Speech Recognition](https://pdfs.semanticscholar.org/d0a1/adf85ef5893b3a84fbb64cca951715e54299.pdf)
[Collocations](https://nlp.stanford.edu/fsnlp/promo/colloc.pdf)
[Hyphens](https://en.oxforddictionaries.com/punctuation/hyphen#hyphens_in_compound_words)