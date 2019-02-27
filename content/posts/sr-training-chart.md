+++ 
title = "Sr Training Chart" 
date = 2019-02-26T21:19:51-06:00 
tags = []
featured_image = "" 
description = "" 
+++

### Training Set
50 audio format sentences from Ted
18 audio format sentences from Astrom

| Model Type | Default | Ted-HMM | Astrom-HMM | Ted+Astrom-HMM | Ted+Astrom-HMM+g2p | Ted+Astom-HMM+openslrG2P |
|------------|---------|---------|------------|----------------|--------------------|--------------------------|
| Ted        | 33.1    | 15.8%   | -          | 19.2%          | 18%                | 16.8%                    |
| Astrom     | ?       | -       | ?          | 30%            | 27.8%              | 39.9%                         |

- Ted-HMM follows the adapting acoustics models step using Ted audio as training file
- Ted+Astrom-HMM+g2p adapted the acoustics models and add extended dictionary according to words not in the first training session
- Ted+Astom-HMM+openslrG2P adapted the acoustics model and uses openslr vocabulary