---
title: "Composing music via Markov's chains"
excerpt: "Using NLP to understand MIDI files"
date: "2021-09-10"
header:
  teaser: /assets/images/programming/midi/test_01_composition.png
sidebar:
  - title: "Language"
    text: "Python"
  - title: "Additional tags"
    text: "midi, music21, markov's chains"
gallery:
---

## Introduction

The aim of this project was to analyse and process data on group of MIDI files with various music pieces in order to create new music compositions. The main focus was harmonic analysis meaning understanding the chordes that occur one after another. The project omits tempo, rythm or instrumentals of the pieces.

The result is a composition that mimics the human composer way of creating music.

In order to achieve it the tools that were used are:
 - Python 3.5
 - Pandas library
 - Music21 Library
 - word2vec neural network
 - Markov Chains implementation

The data that was used was scrapped from vg music and it contains over 30 thousand pieces. To download the data I've written webscrapper wrote in python and list of subdomains of the page with MIDI files. I've managed to get access to 90% of files which is around 27 thousand MIDI files. 


## Data analysis

In order to process the data I've simplfiied the content of the midi files to reduce the data of different instruments as seen below:

![alt text](../../assets/images/programming/midi/pitches_instruments.png)


Into one instrument data as below:

![alt text](../../assets/images/programming/midi/one_instrument.png)

This allows for quicker analysis of progression while losing data about the whole orchestral part of composition, focusing on chords.

I've tested several note analysis algorithms and measured the quantity of pitch presence as presented on graph below.

![alt text](<../../assets/images/programming/midi/pitch class histogram.png>)

Aswell as density of note usage as below.

![alt text](<../../assets/images/programming/midi/time density.png>)

In order to simplify harmonic progression the function "chordify" was used. It allows to convert a singular notes into chordes. The chordes alone don't present the full context, because the key is what determines the first note of an octave. In order to unify te context the music theory was used. All of the compositions were analysed based on their functional chords named with roman numerals I-VII. This allows analysis of only function of the chord within context of key instead of just the chord.

The chords were simplified aswell in order to reduce the vocabulary, so the chordes with high similarity were merged into one chord.

This reduction was contained inside of a dataframe, which head of can be seen below.

![alt text](../../assets/images/programming/midi/head.png)

Records formulated like that were input into word2vec to get the similarity vectors.

![alt text](../../assets/images/programming/midi/similarity.png)


While not intuitive, the results were propable so the next step was to use Markov's chains in order to generate new compositions.

After few modifications of dataset, few phrases were created.

![alt text](../../assets/images/programming/midi/sentences.png)


The functional chords were later interpreted as composition in random key in 4/4. Below are three generated compositions:

Composition 1:

![alt text](../../assets/images/programming/midi/test_01_composition.png)

Composition 2:

![alt text](../../assets/images/programming/midi/test_02_composition.png)

Composition 3:

![alt text](../../assets/images/programming/midi/test_03_composition.png)


## Summary

There's certainly potential in NLP in terms of creating music as long as the data is properly normalized and translated.
While results are not ideal theres plenty of space into further research and optimizations.