+++ 
title = "Using G2p Seq2seq to Extend the Dictionary" 
date = 2019-02-10T14:02:40-06:00 
tags = []
featured_image = ""
description = "" 
+++

## Introduction
When we are adapting-acoustics-models, there are some utterances that fail to produce phonetic transcription.
Those are words not in your model dictionary.
Therefore, we would like to extend our dictionary. 

## Using g2p-seq2seq to extend the dictionary
To keep the consistency, we use CMUSphinx recommend g2p-seq2seq. 

### Installation
git clone the module from the following link: 
https://github.com/cmusphinx/g2p-seq2seq

 ```
 sudo python setup.py install  
 python setup.py test
 ```

* Remember to update or install python setuptools. 

G2P is running on Tensorflow, the version need to be:

* tensor2tensor==1.6.6 (require downgrade)
* g2p-seq2seq==6.2.2a0
* tensorflow==1.13.0rc1 (at least)

### Running G2P 
We are using an English model 2-layer LSTM with 512 hidden units provided by CMUSphinx website.
G2P has an interactive mode:
```
$ g2p-seq2seq --interactive --model_dir ./g2p-seq2seq-model-6.2-cmudict-nostress
...
> hello
...
Pronunciations: [HH EH L OW]
...
>

```
To generate pronunciations for an English word list with a trained model, run:
```
  g2p-seq2seq --decode wordlist.txt --model_dir ./g2p-seq2seq-model-6.2-cmudict-nostress > output_dict.txt
```

### Processing Output dictionary

G2P will output the pronunciations along with a blank sentence.
In order to remove the blank sentences, run this in Terminal:
 ```
 sed -i "" '/^$/d' output_dict.txt
 ```

reference: https://cmusphinx.github.io/wiki/tutorialdict/#using-g2p-seq2seq-to-extend-the-dictionary


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

However, it is not necessary because the dictionary is not complete/representative? enough. 
Human pronunciation such a 'it's-it's', 'an-and', 'the-the' are colloquial duplication.

**How can we fix this issue?** 