---
title: Spacy PhraseMatcher
tags: [NLP,Spacy]
style: border
color: secondary
description: Extracting image address using spacy with PhraseMatcher
---

## Using PhraseMatcher to identify known person names, organizations  and locations from the text chunk

```python
import spacy
import common_module as cm
from spacy.matcher import PhraseMatcher
nlp = spacy.load('en_core_web_sm')
matcher = PhraseMatcher(nlp.vocab)
import string
import pickle
import requests
from bs4 import BeautifulSoup
```

##### Here is the known sample data to matcher


```python
persons = ['john','miky','mikle','mike']
orgs = ['apple','microsoft','nokia','samsung']
locations = ['india','japan','singapore','usa','america','dubai']
```


```python
#Making patterns and adding them to matcher()
persons_patterns = [nlp.make_doc(term) for term in persons]
persons_patterns

# Converted into nlp documents nlp.make_doc used to simplyfy the process
```




    [john, miky, mikle, mike]




```python
orgs_patterns = [nlp.make_doc(term) for term in orgs]
orgs_patterns
```




    [apple, microsoft, nokia, samsung]




```python
locations_patterns = [nlp.make_doc(term) for term in locations]
locations_patterns
```




    [india, japan, singapore, usa, america, dubai]




```python
# Adding the patterns to the matcher
matcher.add("PER", None, *persons_patterns)
matcher.add("ORG", None, *orgs_patterns)
matcher.add("LOC", None, *locations_patterns)
```


```python
TEST_CASES = ['Mike is from India and he is working on samsung office','John working on apple office in USA']
```


```python
def update_output(string_id,text,output):
    if text not in output[string_id]:
        output[string_id].append(text)
        
def match_processor(text):
    document = nlp(text)
    matches = matcher(document)
    output = {'PER':[],"ORG":[],'LOC':[]}
    for match_id, start, end in matches:
        string_id = nlp.vocab.strings[match_id]
        span = document[start:end]
        update_output(string_id,span.text,output)
    return output
```

##### Expected output look like


```python
for sentence in TEST_CASES:
    print(match_processor(sentence))

```

    {'PER': ['Mike'], 'ORG': ['samsung'], 'LOC': ['India']}
    {'PER': ['John'], 'ORG': ['apple'], 'LOC': ['USA']}
    
