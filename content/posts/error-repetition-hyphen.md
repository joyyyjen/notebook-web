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

Repetition should be kept. When deaf people watching the video, we want to keep a consistent between facial/mouth shape and the text
### Number
In our phonetic dictionary, we only have numbers in word form. It conflicts with our reference text.
Should we delete it from the error list? Or can we add the number into the phonetics dictionary?

> I found another phonetics dictionary and it also print number in word forms. The reason we prefer number form is because the reading efficiency. 
One way to solve it is by transferring all the word forms into numbers. The script I got is: 
```
def text2int (textnum, numwords={}):
    if not numwords:
        units = [
        "zero", "one", "two", "three", "four", "five", "six", "seven", "eight",
        "nine", "ten", "eleven", "twelve", "thirteen", "fourteen", "fifteen",
        "sixteen", "seventeen", "eighteen", "nineteen",
        ]

        tens = ["", "", "twenty", "thirty", "forty", "fifty", "sixty", "seventy", "eighty", "ninety"]

        scales = ["hundred", "thousand", "million", "billion", "trillion"]

        #numwords["and"] = (1, 0)
        for idx, word in enumerate(units):  numwords[word] = (1, idx)
        for idx, word in enumerate(tens):       numwords[word] = (1, idx * 10)
        for idx, word in enumerate(scales): numwords[word] = (10 ** (idx * 3 or 2), 0)

    ordinal_words = {'first':1, 'second':2, 'third':3, 'fifth':5, 'eighth':8, 'ninth':9, 'twelfth':12}
    ordinal_endings = [('ieth', 'y'), ('th', '')]

    textnum = textnum.replace('-', ' ')

    current = result = 0
    curstring = ""
    onnumber = False
    for word in textnum.split():
        if word in ordinal_words:
            scale, increment = (1, ordinal_words[word])
            current = current * scale + increment
            if scale > 100:
                result += current
                current = 0
            onnumber = True
        else:
            for ending, replacement in ordinal_endings:
                if word.endswith(ending):
                    word = "%s%s" % (word[:-len(ending)], replacement)

            if word not in numwords:
                if onnumber:
                    curstring += repr(result + current) + " "
                curstring += word + " "
                result = current = 0
                onnumber = False
            else:
                scale, increment = numwords[word]

                current = current * scale + increment
                if scale > 100:
                    result += current
                    current = 0
                onnumber = True

    if onnumber:
        curstring += repr(result + current)

    return curstring
``` 
Notice: 
- We can also transfer number to words during training and convert it back. 

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