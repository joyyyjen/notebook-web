+++ 
title = "Creating Text Corpus From Wiki" 
date = 2019-02-26T16:51:59-06:00 
tags = []
featured_image = "" 
description = "" 
+++

### Speech Recognition consists of 3 main models: 
1. Acoustic Model : acoustic properties for each senone (HMM)
2. Phonetic Dictionary: a mapping from words to phones
3. Language Model: to restrict word search
<ul>What is the next words? 
 <ul>In spite __</ul></ul>
 
In our application, we are working on Speech-to-Text Auto Captioning for Speeches in NCSA (The National Center for Supercomputing Applications ) Talks.
Thus, most of the talks have more related to science fields. The open source speech recognition CMUSphinx has a general models.
In this article, I will present how can we create domain-specific phonetics model. 

### Step 1: Get the Wikipedia Database
Wikipedia offers free copies of all available content to interested users. <br>
 [Wikipedia:Database download](https://en.wikipedia.org/wiki/Wikipedia:Database_download#Help_to_parse_dumps_for_use_in_scripts) lists all the possible tools, databases, and answers to the frequent asked questions. 

To download a subset of the database in XML format, such as a specific category or a list of article, we can use [Special:Export](https://en.wikipedia.org/wiki/Special:Export). Usage of which is described at 
[Wikipedia:Help:Export](https://en.wikipedia.org/wiki/Help:Export).

[Wikipedia:Portal:Contents/Categories](https://en.wikipedia.org/wiki/Portal:Contents/Categories) shows the categories of Wikipedia's content, such as "Culture and the arts", "Geography and places","Human activities" and etc. 

The category I chose are: technology, science, computer, environment, policy, education.
I added each category one by one and got the following namespaces. 
![alt text][img1]
[img1]: https://raw.githubusercontent.com/joyyyjen/notebook-web/master/docs/img/export.png "Wiki:Special:Export"

### Step 2: convert the dump file to sentences

[WikiExtractor](https://github.com/attardi/wikiextractor) is a Python script that extracts and cleans text from a Wikipedia database dump.
The tool is written in Python and requires Python 2.7 or Python 3.3+ but no additional library.

#### Installation
 ```
git clone https://github.com/attardi/wikiextractor.git
(sudo) python setup.py install
 ```

#### Execution

```
cd wikiextractor/
python3 WikiExtractor.py -o ../Output.txt ../Wikipedia-20190222003910.xml 
```
System Output:
![alt text][img2]
[img2]: https://raw.githubusercontent.com/joyyyjen/notebook-web/master/docs/img/extractor.png "wikietractor system output"

Output Result:
![alt text][img3]
[img3]: https://raw.githubusercontent.com/joyyyjen/notebook-web/master/docs/img/wiki00.png "wikietractor output result"

To further clean the text and format it to our desired form, run the following bash command:
```
cat Output.txt | sed 's/<.*>//' | tr -d '\.\,&' | tr ' ' '\n' | sed '/^[[:space:]]*$/d' | tr [[:upper:]] [[:lower:]] > wiki_01.txt 
```
- **sed 's/<.\*>//'** delete everything inside "<>"
- **tr -d '\.\,&'**  and **sed '/^[[:space:]]\*$/d'** delete empty string (I forgot why I repeat twice)
- **tr ' ' '\n'** translates space to newline
- **tr [[:upper:]] [[:lower:]]** translate upper case letter to lower case


More updates...

The article is inspired by [Creating a text corpus from Wikipedia](http://trulymadlywordly.blogspot.com/2011/03/creating-text-corpus-from-wikipedia.html)


