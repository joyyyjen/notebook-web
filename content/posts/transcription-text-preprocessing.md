+++ 
title = "Transcription Text Preprocessing" 
date = 2019-01-26T16:29:38-06:00 
tags = []
featured_image = "" 
description = "" 
+++

## Introduction
There are two file formats for speech recognition training and analysis. If we have srt file for initial transcription file, we need to convert it to plain text format.
We will cover how to srt to plain text in Section 1, then Section 2 discuss how to covert the plain text file we have from Section 1 to acceptable training format.

### Section 1 - SRT to Plain Text
A .srt files looks like this:

> 1
00:00:02,168 --> 00:00:06,005
We are kicking off this evening

>2
00:00:06,005 --> 00:00:10,210
a week-long celebration
of a landmark event,

The basic idea is to find the line that has letter and to check whether the sentence is a complete sentence. 
```
for line in lines:
	# Check if the line has letter 
	if re.search('[a-zA-Z]',line) == None: continue
	# Convert everything to lower case
	line = line.strip('\n').lower()
			# Check whether the line is a complete sentence or not. If it is complete, start a new line
	if line[-1] == '.': 
		line = line.translate({ord(i):None for i in ',."'})
		sentence = sentence + str(line)

		# Save complete sentence to outputFile
		outputFile.write('{}\n'.format(sentence))
		sentence = ""
	else: 
		line = line.translate({ord(i):None for i in ',."'})
		sentence = sentence + str(line) + ' '
```
Output Text:

> we are kicking off this evening a week-long celebration of a landmark event which is the golden anniversary of the allerton conference on communication control and computing

> for those of you who have been regular attendees at the allerton conference for many years and for some probably allerton is special place in your career having presented your first paper or second paper in your career at the allerton conference and i know that there are many of you out there

### Section 2 - Plain Text to Training Transcription format
The training transcription format requires a sentence open tag, close tag, and fileid.

> \<s> we are kicking off this evening a week-long celebration of a landmark event which is the golden anniversary of the allerton conference on communication control and computing \</s> (1)
\<s> for those of you who have been regular attendees at the allerton conference for many years and for some probably allerton is special place in your career having presented your first paper or second paper in your career at the allerton conference and i know that there are many of you out there \</s> (2)

We can get the fileids by the following command:
```
for filename in *.wav ; do echo "${filename%.*}"; done > Astrom.fileids
```
- **"${filename%.\*}"** ignores the file extension

```
for i in range(len(ids)):
    outputFile.write("<s> {} </s> ({})\n".format(lines[i].strip("\n"),ids[i].strip("\n")))

```
