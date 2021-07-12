---
title: PhraseMatcher Email Extraction
tags: [NLP,Spacy]
style: border
color: secondary
description: Extracting image address using spacy with PhraseMatcher
---

Using Spacy to train and extract the email address from the string

```python
from spacy.lang.en import English
from spacy.matcher import PhraseMatcher
import common_module as cm
import requests
nlp = English()
matcher = PhraseMatcher(nlp.vocab, attr="SHAPE")

training_set = ["John.Smith@example.com", "part@domain", "jsmith@example.com", "part@domain", "fred@domain", "fred@domain.", "john.smith@example.com", "john.smith@example.com.", "jsmith@example.com", "JSmith@example.com", "john.smith@example.com", "john.smith@example.com.", "simple@example.com", "very.common@example.com", "fully-qualified-domain@example.com", "user.name@example.com", "example-indeed@strange-example.com", "joeuser@example.com", "foobar@example.com", "Pelé@example.com"]

title_patterns = [nlp.make_doc(term) for term in training_set]
matcher.add("LAW_FIRM", None, *title_patterns)


text = "My Name is dhamodharan and my email address is dhamo@gmail.com"

#response = requests.get('https://en.wikipedia.org/wiki/Email_address').text
#text = BeautifulSoup(response,'html5lib').text

doc = nlp(text)
found_emails = []
for match_id, start, end in matcher(doc):
    found = doc[start:end]
    if "@" in str(found):
        found_emails.append(doc[start:end])
    
print(found_emails)

#Expected Result
#[dhamo@gmail.com]
```
