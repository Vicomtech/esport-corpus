

# ES-PORT CORPUS
Released May, 2018

* * *


1. [Introduction](#introduction)
2. [Data structure](#data-structure)
3. [Annotation and events description](#annotation-and-events-description)
    3.1. [Language events](#language-events)
    3.2. [Pronunciation, noise, and silence events](#pronunciation-noise-and-silence-events)
    3.3. [Other annotations](#other-annotations)
4. [License](#license)
5. [Reference](#reference)
6. [Download](#download)
7. [Contact](#contact)

* * *


## INTRODUCTION


The Spanish Technical Support (ES-Port) Corpus is a spontaneous spoken human-
human dialogue corpus consisting of dialogues from calls to the technical 
customer support service of a Spanish telecom operator for companies. The 
corpus has been directly transcribed from call recordings, annotated at various
linguistic and acoustic-related extralinguistic levels, and anonymised in order
to comply with data protection legislation.

The corpus consists of 1170 dialogue transcriptions and has around 535k tokens
(punctuation marks not included), with a vocabulary size of around 11200 words.
It also contains around 3000 language switch events and approximately other
11500 noise, pronunciation, and silence related events. Other phenomena like
word lengthening or shortening, false starts, repetitions, nonwords, and some
recurrent filler words and continuers have also been annotated.


***


## DATA STRUCTURE


Dialogues are structured in JSON format files. Each file contains a "file-id"
corresponding to the dialogue's identification name and a "turns" list of the
turns that form the given dialogue.

Each turn contains the following data:

- "turn": ordered turn index in the dialogue (integer)
- "labels": a list of dictionaries including the individual words which
have been anonymised and their label (list) [
     - {
        - "word" : the anonymised word (string), 
        - "label": its label (string)
        
        }
        
    ]

- "speakers": each of the speakers in the given turn and a list of their
utterances (dictionary) {
    - speaker id (string): (list) [
        - {"utterance": transcription (string)}
        
        ]
    
    }
    
- "filtered-text": raw transcription without event annotations (string)

- "text-events": complete utterance plus events transcription in  their 
order of occurrence. Events and language switching are annotated using 
@ and square brackets (string)

- "language-events": a list with the language switching events
in the turn (list) [
    - {
        - "speaker": speaker id (string),
        - "language-name": language code (string),
        - "description": transcription of the non-Spanish words (string)
       
        }
       
    ]

- "events": a list of the events (other than language switch) in the turn
(list) [
    - {
        - "speaker": speaker id (string),
        - "name": event type (string),
        - "description": transcription of the words co-occurring with the
         event or |Null| if none (string)
         
        }
        
    ]



Turns are segmented into different utterances when there is a language
switch or other event, when there is an overlap, or in cases when a pause
of more than 100ms and less than 200ms takes place. If the pause is longer
than 200ms the turn is ended.


Here is a real example of a turn's structure:

    {
            "events": [
                {
                    "speaker": "spk2",
                    "name": "noise-rire",
                    "description": "|Null|"
                }
            ],
            "speakers": {
                "spk2": [
                    {
                        "utterance": "Sí, sí, en el"
                    },
                    {
                        "utterance": "router"
                    },
                    {
                        "utterance": "hay una= pegatina que pone Efeledos"
                    }
                ]
            },
            "turn": 10,
            "labels": [
                {
                    "label": "organisation",
                    "word": "Efeledos"
                }
            ],
            "language-events": [
                {
                    "speaker": "spk2",
                    "description": "router",
                    "language-name": "en"
                }
            ],
            "text-events": "Sí, sí, en el @[*LANGUAGE*: en(router)-spk2] hay una= pegatina que pone Efeledos @[*EVENT*: noise-rire(|Null|)-spk2]",
            "filtered-text": "Sí, sí, en el router hay una= pegatina que pone Efeledos"
    }


***



## ANNOTATION AND EVENTS DESCRIPTION

### LANGUAGE EVENTS


Language events in the corpus indicate a switch to a language other than
Spanish by the speaker. They are only annotated when the speaker pronounces
the given word(s) correctly in the target language. Otherwise they are left
unannotated if pronounced as if they were Spanish, or annotated with a
mispronunciation event if pronounced freely.

The other languages occurring in the ES-Port corpus are: Basque (eu), Catalan
(cat), Asturian (ast), French (fr), Italian (it), and English (en). The latter
is the most frequent one, reaching up to 91.59% of language events occurrences.



### PRONUNCIATION, NOISE, AND SILENCE EVENTS


Other events in the corpus can be of type *noise*, *pronounce*, or *silence*.
These were annotated during the corpus transcription phase and therefore follow
the conventions of the tool used for that purpose: Transcriber 1.5 [^cita].
The ones appearing in the corpus are briefly described bellow:


[^cita]: Barras, C., Geoffrois, E., Wu, Z., and Liberman, M. (2001). Transcriber: development and use of a tool for assisting speech corpora production. Speech Communication, 33(1):5–22.


- Pronounce:

    - pi:  one or two words unintelligible.
    - pif:  one or two words unintelligible due to low volume.
    - ch:  whispered.
    - *:  word is intelligible but mispronounced.


- Noise:

    - nontrans:  segment unintelligible.
    - b:  indeterminate noise.
    - bb:  mouth noise.
    - bg:  throat noise.
    - shh:  electric noise.
    - r:  breathing.
    - i:  inhalation.
    - e:  exhalation.
    - pf:  blast of air.
    - n:  sniffing.
    - tx:  cough, throat clearing, sneeze.
    - sif:  whistling.
    - rire:  laughter.
    - rire en fond:  background laughter.
    - toux en fond:  background cough.
    - conv:  background conversation.
    - musique:  music.


- Silence.



### OTHER ANNOTATIONS


Other annotations in the corpus include:

- unfinished words and nonwords:  marked with "< ->" symbols surrounding
the word.
- repetitions and false starts:  marked with "< >" symbols surrounding the
word or words.
- lengthening in pronunciation:  marked with a "=" symbol at the end of
the word.
- typical Spanish shortenings in pronunciation:  marked with a "+" symbol
at the beginning of the word.
- some continuers and filler words:  marked with "<% >" symbols surrounding
the item. When fillers consist of more than one word (e.g. "o sea"), these
are written altogether (e.g. "<%osea>").


Here is a complete list of the continuers and fillers labelled in the corpus
and what they imply:

- Doubt, stall:  
    - eh         
    - ah             
    - ahm              
    - ehm                              
                              
- Check, request repetition:   
    - ehh                            
    - hm                     
                   
- Thinking:       
    - mm                     
    - mmm                 
                        
- Assertion:        
    - mhm                         
    - aha                         
    - ahaha               
            
- Negation:   
    - nm         
                 
- Annoyance, error:    
    - uhm

- Realisation:       
    - ahh

- Multipurpose and others:     
    - ay        
    - oy             
    - bua                  
    - buff                   
    - puff                 
    - uff               
    - a ver           
    - o sea           
    - en fin           
    - bueno          
    - hala           
    - hale             
    - mira            
    - mire              
    - vamos            
    - venga            
    - hombre             
    - ostras           
    - sabe          
    - sabes           
    - pues          
    - oye       


***

## LICENSE


The resources in this repository are licensed under the Creative Commons Attribution-ShareAlike 3.0 Spain
License. To view a copy of this license, visit http://creativecommons.org/licenses/by-sa/3.0/es/ or send
a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.


***

## REFERENCE


If you use this corpus, please cite the following [paper](https://aclanthology.org/L18-1125.pdf):

```
García-Sardiña, L., Serras, M., and del Pozo, A. (2018). ES-Port: a 
Spontaneous Spoken Human-Human Technical Support Corpus for Dialogue 
Research in Spanish. In *LREC*.
```

For any further information about the corpus, please refer to the mentioned
paper.


***


## DOWNLOAD

Download link: https://datasets.vicomtech.org/di01-esport-corpus/esport-corpus.zip


## CONTACT

language@vicomtech.org

