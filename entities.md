# Entities

[github.com/deepchar/entities](https://github.com/deepchar/entities/) *Transliteration for translation of named entities*

The **[transliteration](https://deepchar.github.io/)** for translation of **[named entities](https://en.wikipedia.org/wiki/Named_entity)** is a different task than the transliteration from informal variants back into their canonical forms.  Instead of many inputs mapping to one output, **one input will map to many outputs.** 

Transliteration of named entities is useful in search engines, especially within social networks, and for legal discovery, criminal investigations and financial or governmental applications.  The goal is to find all possible versions of name, in all alphabets. 

The same name referring to the same person or other entity could be indexed or queried in multiple forms:

**```Ершов```**, ```Yershov```, ```Ershov```, ```Jerszow```,...

**```Шостакович```**, ```Shostakovich```, ```Šostakovič```, ```Xostakóvitx```, ```Szostakowicz```,...

**```Թագուշ```**, ```Tagoush```, ```Tagoosh```, ```Tagush```, ```Tagusch```, ```Taguš```, ```Тагуш```, ...

**```Alegre```**, ```Алегри```, ```Ալեգրի```, ```Ալեգրե```, ```آلگری```, ```Αλέγκρε```...


These forms are not standardised, but they are also not typos or misspellings.  Certain forms do not occur organically: *```Tagus```, ```Тагусх```*, ...

A human literate in the relevant languages and alphabets generally maps them back to the canonical form correctly and effortlessly, subconsciouly resolving ambiguity and discarding invalid candidates: *```Ершев```, ```Ерщов```, ```Թագոուշ```, ```Տագուշ```, ```Թաքուշ```, ```Ершёв```, ```Схостаковицх```*, ...

Our initial task is canonicalisation: **given an informal child form and a target language and script, generate the canonical form.**

```Yershov``` + ```ru-Cyrl``` ➜ **```Ершов```**  

```Ershov``` + ```ru-Cyrl``` ➜ **```Ершов```**  

```Tagoush``` + ```hy-Armn``` ➜  **```Թագուշ```**

```Tagush``` + ```hy-Armn``` ➜  **```Թագուշ```**

```Tagoush``` + ```ru-Cyrl``` ➜  **```Тагуш```**

```Алегри``` + ```pt-Latn```➜  **```Alegre```**

```Алегри``` + ```en-Latn```➜  **```Alegre```**

```Ալեգրի``` + ```pt-Latn```➜  **```Alegre```**

Our initial target languages are **Russian**, **Armenian**, **Persian** and **Greek**, and our source script is generally the Latin alphabet.

We evaluate the output with simple exact-match **accuracy** and **character error rate (CER)**, based on [word error rate](https://en.wikipedia.org/wiki/Word_error_rate) (WER) but adapted for character-level sequence generation tasks.

We could also formulate the task in reverse: given a canonical form, generate child forms.


### Types of name transliteration

Firstly we should consider the processes by which canonical forms are converted into their various child forms.

Many cases are fairly simple.

```Հովիկ Աբրահամյան``` ➜ ```en-Latn: Hovik Abrahamyan```  
```Հովիկ Աբրահամյան``` ➜ ```pl-Latn: Hovik Abrahamian```  
```Հովիկ Աբրահամյան``` ➜ ```de-Latn: Hovig Abrahamyan```  
```Հովիկ Աբրահամյան``` ➜ ```ru-Cyrl: Овик Абраамян```  
```Հովիկ Աբրահամյան``` ➜ ```uk-Cyrl: Овік Абрамян```  
```Հրազդան``` ➜ ```en-Latn: Hrazdan```  
```Հրազդան``` ➜ ```ru-Cyrl: Раздан```  
```Tom Collins``` ➜ ```ru-Cyrl: Том Коллинз```  

Some names can have dozens of versions when transliterated into another language.

````Armn: Շողիկ Հովհաննեսի Ցոլակյան```` ➜ ````Latn: Shoghik Hovhannes Tsolakyan, Shokhik Hovhanes Tsolakian, Shoghig Hovhaness Tzolakyan````

There are of course multiple languages per script, and some of the various is due to the specific target language.

````Armn: Շողիկ Հովհաննեսի Ցոլակյան```` ➜ ````pl-Latn: Szochik Hovhaness Colakian````

````Armn: Հովիկ Աբրահամյան```` ➜ ````pl-Latn: Howik Abrahamian````  
````Armn: Հովիկ Աբրահամյան```` ➜ ````de-Latn: Howik Abrahamjan````  

````Armn: Սերժ Սարգսյան```` ➜ ````en-Latn: Serzh Sargsyan````  
````Armn: Սերժ Սարգսյան```` ➜ ````fr-Latn: Serge Sarkissian````  
````Armn: Սերժ Սարգսյան```` ➜ ````hr-Latn: Serž Sargsjan````  
````Armn: Սերժ Սարգսյան```` ➜ ````nl-Latn: Serzj Sarkisian````  

````Latn: John Smith```` ➜ ````hy-Armn: Ջոն Սմիթ````  
````Latn: John Smith```` ➜ ````ru-Cyrl: Джон Смит, sr-Cyrl: Џон Смит, uk-Cyrl: Джон Сміт````  
````Latn: John Smith```` ➜ ````ar-Arab:  جون سميث, fa-Arab: جان اسمیت````    
  
````Latn: SnapChat```` ➜ ````hy-Armn: Սնապչատ````  
````Latn: SnapChat```` ➜ ````ru-Cyrl: Снепчат, bg-Cyrl: Снапчат````  
````Latn: SnapChat```` ➜ ````tr-Latn: Snapçat, pl-Latn: Snapczat````  

#### Multiple conversions

Quite often, a name could be repeatedly transliterated:

````Հովիկ Աբրահամյան```` ➜  ````ru-Cyrl: Овик Абраамян```` ➜ ````en-Latn: Ovik Abramyan````  
````Հրազդան```` ➜ ````ru-Cyrl: Раздан```` ➜ ````en-Latn: Razdan````  
````Howard Hughes```` ➜ ````ru-Cyrl: Говард Хьюз```` ➜ ````hy-Armn: Գովարդ Խյուզ````  
```Шостакович``` ➜ ```it-Latn: Šostakovič```  ➜ ```en-Latn: Sostakovic```

It could even come back into the original script mangled.

````Latn: Howard Hughes```` ➜ ````ru-Cyrl: Говард Хьюз```` ➜ ````en-Latn: Govard Khyuz````  
````Latn: Charles Aznavour```` ➜ ````ru-Cyrl: Шарль Азнавур```` ➜ ````en-Latn: Sharl Aznavur````  
````Armn: Հասմիկ Կուրղինյան```` ➜ ````ru-Cyrl: Асмик Кургинян```` ➜ ````hy-Armn: Ասմիկ Կուրգինյան````   


#### Within alphabets

We can also consider some variants within the alphabet.

There are three common transcriptions for German [umlaut](https://en.wikipedia.org/wiki/Germanic_umlaut).

1. Preserve the umlaut   
````Müller```` ➜ ````Müller````
2. Decompose the diacritic  
````Müller```` ➜ ````Mueller````  
3. Simply omit the umlaut  
````Müller```` ➜ ````Muller````  

Umlaut decomposition is specific to German.  In many other orthographic traditions, the umlaut is usually simply omitted.

Some Latin orthographies nativize according to pronunciation, not unlike non-Latin alphabet orthographies.

````George Bush```` ➜ ````tr-Latn: Corc Buş````  
````George Bush```` ➜ ````de-Latn: Schorsch Busch````  
````George Bush```` ➜ ````sq-Latn: Xhorxh Bysh````  


## Our approach

We use an approach similar to those commonly used for translation and also our main transliteration project: **parallel corpora**.

We have trained initial models with two different architectures, **seq2seq** and **tensor2tensor**.


## Datasets

There are very limited datasets available for transliteration, let alone transliteration of named entities in all directions.

Our goal was to create an almost fully automatic data harvesting pipeline so that other researchers in the field of [NMT](https://en.wikipedia.org/wiki/Neural_machine_translation) could use it. We were motivated by the fact that at the time of this research there were very few open sources.  We extracted raw data, learned word alignment and created a dataset of multi-token name pairs.

We assumed that the pronunciation of a last name is independent of the previous tokens, and we and represent **parallel corpora** as pairs of single tokens, as in [Yuval Merhav's and Steven Ash's  approach](https://arxiv.org/pdf/1808.02563.pdf).

| Dataset  | Total size | Training size  | Source Alphabet size | Target Alphabet size | 
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| Latn ➜ hy-Armn  | 39,707  | 25,412  | 152  | 107  |
| Latn ➜ ru-Cyrl  | 179,853  | 143,882  | 278  | 124  |
| Latn ➜ el-Grek  | 37,505  | 30,004  | 170  | 97  |
| Latn ➜ fa-Arab  | 78,663  | 62,930  | 170  | 117  |

The data in the Latin script included not just English but other European languages.

## Baselines

As benchmarks we considered the transliteration module of [Polyglot](https://pypi.org/project/polyglot/) and the bi-directional transliteration library [transliterate](https://pypi.org/project/transliterate/).

## Training

**TODO: describe hardware, training time, hyperparams**

## Results

We compared **[word error rate (WER)](https://en.wikipedia.org/wiki/Word_error_rate)** between Polyglot, and our seq2seq and tensor2tensor implementations on the given parallel corpora.

The results of the [transliterate](https://pypi.org/project/transliterate/) library baseline are not shown because they were not competitive at all.

Interestingly, the **seq2seq model consistently performed better** than the tensor2tensor model. The only exception is Latn ➜ el-Grek, where seq2seq and tensor2tensor had equal results.  Moreover, the tensor2tensor model failed to train on the Persian dataset.

## WER

| Target language  | Polyglot baseline | seq2Seq  | tensor2Tensor | 
| :-------------: | :-------------: | :-------------: | :-------------: |
| hy-Armn  | 0.64  | **0.46**  | 0.47  |
| ru-Cyrl  | 0.56  | **0.24**  | 0.37  |
| el-Grek  | 0.89  | **0.52** | **0.52**  |
| fa-Arab  | 0.65  | **0.49**  | -  |

## Exact match accuracy

| Target language  | Polyglot baseline | seq2Seq  | tensor2Tensor | 
| :-------------: | :-------------: | :-------------: | :-------------: |
| hy-Armn  | 36.84%  | **77.46%**  | 75.4%  |
| ru-Cyrl  | 43.93%  | **90.04%**  | 77.73%  |
| el-Grek  | 10.99%  | **73.33%** | 66.5%  |
| fa-Arab  | 35.45%  | **73.96%**  | -  |

## Sample outputs

For each target language and thus for each model, we show a few examples where the model did well - the top candidate was the correct output - and a few where the model did poorly - the correct output was not even among the top 3 candidates - and in that case we also include the actual correct output.

#### hy-Armn

| Source text | Correct output | Model outputs | [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) |
| :-------------: | :-------------: | :-------------: | :------: |
| chukhajyan | `չուխաջյան` | **չուխաջյան**<br/>չուխայան<br/>ճուխաջյան<br/>...  |  0<br/>1<br/>1<br/>... | 
| okroyants | `օքրոյանց` | օկրոյանց<br/>**օքրոյանց**<br/>օխրոյանց<br/>...  |  1<br/>0<br/>1<br/>... |
| yeray  | `երայ` | յերայ<br/>էրայ<br/>**երայ**<br/>...  |  0<br/>1<br/>1<br/>... | 
| gobelyan | `կոպելյան` | գոբելյան<br/>գոբիլյան<br/>գոպելյան<br/>...  |  2<br/>3<br/>1<br/>... | 
| dutroux | `դյուտրու` | դուտրու<br/>դութրու<br/>դութրյու<br/>...  |  1<br/>2<br/>3<br/>... | 
| chkheidze  | `չխեիձե` | չկխեյձե<br/>չկխիձե<br/>չխայձե<br/>...  |  2<br/>2<br/>2<br/>... | 

#### ru-Cyrl

| Source text | Correct output | Model outputs | [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) |
| :-------------: | :-------------: | :-------------: | :------: |
| afanasyeva  | `афанасьева` | **афанасьева**<br/>афанасиева<br/>афанасева<br/>...  |  0<br/>1<br/>1<br/>... | 
| bobylev  | `бобылёв` | бобылев<br/>**бобылёв**<br/>бодылёв<br/>...  |  1<br/>0<br/>1<br/>... | 
| savostyanov  | `савостьянов` | савостянов<br/>савостианов<br/>**савостьянов**<br/>...  |  1<br/>2<br/>0<br/>... | 
| suzdal  | `суздаль` | суздал<br/>сюздаль<br/>сюздал<br/>...  |  0<br/>1<br/>1<br/>... | 
| fargère  | `фаржер` | фарджер<br/>фаргер<br/>фарджир<br/>...  |  0<br/>1<br/>1<br/>... | 
| neftegorsk  | `нефтегорск` | нефтегоргк<br/>нефтегорг<br/>нефтегорк<br/>...  |  1<br/>2<br/>1<br/>... | 

#### el-Grek

| Source text | Correct output | Model outputs | [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) |
| :-------------: | :-------------: | :-------------: | :------: |
| kioussis  | `κιούσης` | **κιούσης**<br/>κιούσσης<br/>κιούσις<br/>...  |  0<br/>1<br/>1<br/>... |
| vagelis  | `βαγγέλης` | βαγέλης<br/>**βαγγέλης**<br/>βαγκέλης<br/>...  |  1<br/>0<br/>1<br/>... |
| laguna  | `λαγούνα` | λαγκούνα<br/>λαγκύνα<br/>**λαγούνα**<br/>...  |  1<br/>1<br/>0<br/>... |
| nissiotis  | `νησιώτης` | νισιώτης<br/>νισσιώτης<br/>νυσιώτης<br/>...  |  1<br/>2<br/>1<br/>... |
| dallas  | `ντάλας` | ντάλλας<br/>ντέιλας<br/>ντόλας<br/>...  |  1<br/>2<br/>1<br/>... |
| mcclain  | `μακκλέιν` | μακλάιν<br/>μακλέν<br/>μακλέιν<br/>...  |  2<br/>2<br/>1<br/>... |

#### fa-Arab

| Source text | Correct output | Model outputs | [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) |
| :-------------: | :-------------: | :-------------: | :------: |
| momayez  | `ممیز`<br/> | **ممیز**<br/>ممیظ<br/>معمیز<br/>...  |  0<br/>1<br/>1<br/>... |
| fourie  | `فوری`<br/> | فوریه<br/>**فوری**<br/>فوریا<br/>...  |  1<br/>0<br/>1<br/>... | 
| o'donnell  | `اودانل`<br/> | اودنل<br/>اودونل<br/>**اودانل**<br/>...  |  1<br/>1<br/>0<br/>... | 
| ligt  | `لیخت`<br/> | لیگت<br/>لیجت<br/>لیگ<br/>...  |  1<br/>1<br/>2<br/>... | 
| entezam  | `انتظام`<br/> | انتزام<br/>انتجام<br/>انتزم<br/>...  |  0<br/>1<br/>1<br/>... | 
| ereyahi  | `الریاحی`<br/> | اریاهی<br/>اریهای<br/>اریهی<br/>...  |  2<br/>3<br/>3<br/>... | 


## Future work

#### More directions
Creating models for different combination of source and target languages, i.e. Latn ➜ ru-Cyrl, Cyrl ➜ en-Latn, Armn ➜ ru-Cyrl, etc.

#### More script types
Creating models for scripts that use ideograms, syllabaries, such as Chinese, Japanese and Korean, or often omit vowels, such as Hebrew or Arabic.

#### Learning curve
Test and graph how dataset size correlates with accuracy

#### Quality and confidence
Research how model errors and quality correlate with token frequency and length.

#### Context
Train models that use context, both the previous or next word or an entire sentence.

#### Architectures
Train character-based NMT [(Ling et al., 2015)](https://arxiv.org/pdf/1808.02563.pdf) or convolutional seq2seq [(Gehring et al., 2017)](https://arxiv.org/pdf/1705.03122.pdf)

#### Massive multilingual model
Train a single general model for all the languages and transliteration directions.


### References

> *Design Challenges in Named Entity Transliteration* 2018  [arXiv](https://arxiv.org/abs/1808.02563)  
> Yuval Merhav, Stephen Ash   
> Amazon Alexa AI, Amazon AWS AI
