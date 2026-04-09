# Tamil Phrase Break Prediction

A phrase break prediction system for Tamil Text-to-Speech (TTS), developed as part of a **MEITY-funded internship project**. Given a raw Tamil sentence, the model assigns each word a prosodic break label — enabling natural pause placement in synthesised speech.

> **Authors:** Nighil Natarajan and Pranav B

## Overview

Phrase break prediction is a sequence-labelling task: given a sequence of Tamil words, assign each word one of four prosodic break labels that govern where a TTS synthesiser introduces pauses and modulates pitch.

| Label | Full Form | Meaning | Typical Context |
|-------|-----------|---------|-----------------|
| `NSIL` | No Silence | No prosodic boundary | Within a phrase or clause |
| `SSIL` | Short Silence | Minor break | Short phrase boundaries |
| `MSIL` | Medium Silence | Moderate break | Between sub-clauses |
| `LSIL` | Large Silence | Major prosodic break | End of clause or complex NP |

## Pipeline Architecture

![image alt](https://github.com/Pranav-B-0071/Phrase_Break_Prediction/blob/91c1f9c07bf7e9dd12c3b47b150e6cf00683d146/Pipeline_Architecture.jpg)

## Features

Each word is represented by the following feature vector:

| Feature | Description |
|---------|-------------|
| `word.lower` | Lowercased word form |
| `word_length` | Character count |
| `pos` | Universal POS tag from Stanza |
| `prefix1/2/3` | First 1–3 characters (agglutinative prefix patterns) |
| `suffix1/2/3` | Last 1–3 characters (Tamil morpheme / case suffix cues) |
| `is_start / is_end` | Sentence boundary flags |
| `prev_word / prev_pos` | Left-context word and POS |
| `next_word / next_pos` | Right-context word and POS |

Tamil is an agglutinative language — grammatical case, tense, and aspect are primarily encoded in **suffixes**. The suffix features serve as a direct proxy for end-syllable morphology, which correlates strongly with prosodic boundary placement.

## Model

A **Conditional Random Field (CRF)** is used, trained with the following configuration:

- **Algorithm:** L-BFGS
- **L1 regularisation (c1):** 0.1
- **L2 regularisation (c2):** 0.1
- **Max iterations:** 500
- **all_possible_transitions:** True

CRFs jointly model the entire label sequence, capturing transition probabilities between adjacent labels — essential for sequential prosodic boundary prediction.

## Results

- **Test Accuracy:** 96.19%
- **Train Accuracy:** 98.52%
