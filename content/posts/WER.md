+++ 
title = "Word Error Rate Algorithm" 
date = 2019-02-13T14:53:07-06:00 
tags = []
featured_image = "" 
description = "" 
+++
## WER

Words error rate is a common metric of the performance of a speech recognition.

The formula is WER = ( I + D + S) / N.

Given an original text, a recognition text with a length of N words,

- S: number of substitutions
- D: number of deletions
- I: number of insertions
- N: total number of words

* The WER is derived from the Levenshtein distance, working at the word level instead of the phoneme level. 

### Levenshtein Distance

Levenshtein distance is used to calculate the minimum steps.
For instance, a reference string "abcdef" and a hypothsis string "azced" has three minimum difference steps.

|   |   | a | b | c | d | e | f |
|---|---|---|---|---|---|---|---|
|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| a | 1 | **0**| 1 | 2 | 3 | 4 | 5 |
| z | 2 | 1 | **1** | 2 | 3 | 4 | 5 |
| c | 3 | 2 | 2 | **1** | **2** | 3 | 4 |
| e | 4 | 3 | 3 | 2 | 2 | **2** | 3 |
| d | 5 | 4 | 4 | 3 | 2 | 3 | **3** |

- Substitute: d[i-1]d[j-1] + 1
- insert: d[i]d[j-1] + 1 (up, down)
- delete: d[i-1]d[j] + 1 (right, left)

We can trace back the result from the bottom most right cell, as indicated by the bold numbers. 

f &rarr; d <br>
d x <br>
b &rarr; z



### Modification in WER on word level
|         |   | their | fresh | new | results |
|---------|---|-------|-------|-----|---------|
|         | 0 | 1     | 2     | 3   | 4       |
| their   | 1 | **0**     | 1     | 2   | 3       |
| first   | 2 | 1     | **1**     | 2   | 3       |
| few     | 3 | 2     | 2     | **2** | 3       |
| results | 4 | 3     | 3     | 3   | **2**   |

new &rarr; few <br>
fresh &rarr; first

|        |   | what | extent |
|--------|---|------|--------|
|        | **0**| 1    | 2      |
| what's | 1 | **1**    | 2      |
| the    | 2 | **2**    | 2      |
| extent | 3 | 3    | **2** |

reference: this youtube video has nice explanation :
https://www.youtube.com/watch?v=We3YDTzNXEk

python code for WER and alignment:
https://github.com/zszyellow/WER-in-python