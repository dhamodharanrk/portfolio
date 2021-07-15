---
title: Getting Started with Natural Language Processing (NLP)
tags: [NLP]
style: border
color: info
description: A basic introduction covers basic steps involved in NLP
---

Source: [way2learnings](https://sites.google.com/view/way2learnings/nlp)

# Named Entity Extraction using NLTK

## What is NLTK?
NLTK is a leading platform for building Python programs to work with human language data. It provides easy-to-use interfaces to over 50 corpora and lexical resources such as WordNet, along with a suite of text processing libraries for classification, tokenization, stemming, tagging, parsing, and semantic reasoning, wrappers for industrial-strength NLP libraries

###### Objective :  Find the Job tile from the profile description

## Steps Involved

#### 1. Importing required Libraries

    from nltk.corpus import stopwords
    from nltk.stem.wordnet import WordNetLemmatizer
    from nltk.tokenize import word_tokenize
    from nltk.tokenize import RegexpTokenizer
    from nltk import pos_tag
    stop_words = set(stopwords.words('english'))
    
    sample_text ='''As head of TLT's Digital group, James co-ordinates the development of the firm's services to organisations in the technology, media and telecoms sectors. He advises on technology development and licensing (including OEM/VAR and similar distribution arrangements); ICT procurement, outsourcing, managed service and cloud based solutions and data privacy.'''

#### 2. Removing Stopwords/Noise from the data
Any piece of text which is not relevant to the context of the data and the end-output can be specified as the noise.

    def get_tokenized(string:str,ignore_stopwords:bool=False):
        word_tokens = word_tokenize(string)
        if ignore_stopwords:
            filtered_sentence = [w for w in word_tokens if not w in stop_words]
            return filtered_sentence
        else: return word_tokens
    
    filtered_sentence = get_tokenized(sampleText,True)

#### 2: Lexicon Normalization
**Lemmatization:** Lemmatization, on the other hand, is an organized & step by step procedure of obtaining the root form of the word, it makes use of vocabulary (dictionary importance of words) and morphological analysis (word structure and grammar relations).

    def get_lemmatize_data(tokens:list):
        lem = WordNetLemmatizer()
        return [lem.lemmatize(w) for w in tokens]
		
    lemmatize_data = get_lemmatize_data(filtered_sentence)

#### 3.  Object Standardization
To make the words in standard formates. For example **msg** will be replaced by **message**

    def get_standardize_words(tokens:list,lookup_dict:dict):
        new_words = []
        for word in tokens:
            if word.lower() in lookup_dict:
                word = lookup_dict[word.lower()]
            new_words.append(word)
        return new_words
    
    lookup_dict = {'rt':'Retweet', 'dm':'direct message', "awsm" : "awesome", "luv" :"love"}
    standardize_words = get_standardize_words(lemmatize_data,lookup_dict) 

#### 4. N-Grams as Features
A combination of N words together are called N-Grams. N grams (N > 1) are generally more informative as compared to words (Unigrams) as features. Also, bigrams (N = 2) are considered as the most important features of all the others. The following code generates bigram of a text.

    tokenizer = RegexpTokenizer(r'\w+')
    pretokenized=tokenizer.tokenize(" ".join(standardize_words))
    print(pretokenized)
    def generate_ngrams(PreTokenized,n):
        output = []
        for i in range(len(PreTokenized)-n+1):
            output.append(PreTokenized[i:i+n])
        return output
    print(generate_ngrams(pretokenized,2))

## Matching Process

#### Direct Matching

Matching the cleaned data againt pre-processed/identified terms.
    
	preIdentified = ['Corporate Finance','Antitrust, competition and trade, Antitrust litigation, Antitrust litigation', 'Construction and engineering, Corporate and M&A', 'Corporate and M&A, Regulatory', 'Payment systems and digital commerce', 'Mixed Use', 'Climate change, Major projects', 'Antitrust, competition and trade, Global investigations, Regulatory', 'Antitrust, competition and trade, Disputes, litigation and arbitration, Regulatory']
    
    def get_direct_match(preIdentified:list,paragraph:str):
        matches = {i.strip():str(paragraph).lower().strip().count(str(i).lower())
                   for i in preIdentified if str(paragraph).lower().count(str(i).lower()) > 0 and len(i.split())> 0}
        focus_Areas = []
        for i in matches.keys():
            if str(i).capitalize() not in focus_Areas:
                focus_Areas.append(str(i).capitalize())
        return focus_Areas

