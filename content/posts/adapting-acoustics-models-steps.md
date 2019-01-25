---
title: "adapting-acoustics-models-steps"
date: 2019-01-05T19:28:37+08:00
---


### 1. Generating acoustic feature files

 ```md
sphinx_fe -argfile ../TrainingSet/acoustic-model/feat.params \
-samprate 16000 -c ted.fileids \
-di . -do . -ei wav -eo mfc -mswav yes 
```

- audio file format must be mono 16k
can use the following bash commend to change the format


```md
for filename in *.wav; do 
ffmpeg -i "$filename" -ac 1 -ar 16000 ./Test1/"$filename"; done
```
Reference Screenshot:
![alt text][img1]
[img1]: https://raw.githubusercontent.com/joyyyjen/notebook-web/master/docs/img/acoustics-feature.png "Acoustic-feature Text"


### 2. Accumulating observation counts
```md
../TrainingSet/bw \
 -hmmdir ../TrainingSet/acoustic-model\
 -moddeffn ../TrainingSet/acoustic-model/mdef.txt\
 -ts2cbfn .ptm. \
 -feat 1s_c_d_dd \
 -svspec 0-12/13-25/26-38 \
 -cmn current \
 -agc none \
 -dictfn ../TrainingSet/cmudict-en-us.dict\
 -ctlfn ted.fileids \
 -lsnfn tag-ted.transcription \
 -accumdir .
```
 Reference Screenshot:
![alt text][img2]
[img2]: https://raw.githubusercontent.com/joyyyjen/notebook-web/master/docs/img/bw.png "bw Text"
 
- If a word is not in the dictionary model, it will not be able to produce phonetic utterance; therefore, skip the utterance

![alt text][img3]
[img3]: https://raw.githubusercontent.com/joyyyjen/notebook-web/master/docs/img/counting.png "accumulating count"

### 3. Updating the acoustic model files with MAP
```md
cp -a ../TrainingSet/acoustic-model en-us-adapt 
<br>
../TrainingSet/map_adapt \
    -moddeffn ../TrainingSet/acoustic-model/mdef.txt \
    -ts2cbfn .ptm. \
    -meanfn ../TrainingSet/acoustic-model/means \
    -varfn ../TrainingSet/acoustic-model/variances \
    -mixwfn ../TrainingSet/acoustic-model/mixture_weights \
    -tmatfn ../TrainingSet/acoustic-model/transition_matrices \
    -accumdir . \
    -mapmeanfn en-us-adapt/means \
    -mapvarfn en-us-adapt/variances \
    -mapmixwfn en-us-adapt/mixture_weights \
    -maptmatfn en-us-adapt/transition_matrices
```
Reference Screenshot:
![alt text][img4]
[img4]: https://raw.githubusercontent.com/joyyyjen/notebook-web/master/docs/img/map-adopt.png "map-adapt text"

reference: https://cmusphinx.github.io/wiki/tutorialadapt/