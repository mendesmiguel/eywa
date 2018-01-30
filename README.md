# eywa
Open source framework for building conversational agents [WIP]

## The `Document` is used to represent strings. It does smart tokenization and entity extraction for known types: date/time, number, phone number, email, url.

```python
>>> from eywa.lang import Document

x = 'my dob is dec 18 1996'
doc = Document(x)
'''
>>> x
my dob is dec 18 1996
           DateTime
'''

# print tokens and their types

for token in doc:
    print(token, token.type)

'''
(my, None)
(dob, None)
(is, None)
(dec 18 1996, 'DateTime')
'''

# Get a python `datetime.datetime` object from the last token:
print(doc[-1].entity.data)

'''
datetime.datetime(1996, 12, 18, 0, 0)
'''

```

## Classifier

```python
from eywa.nlu import Classifier
from eywa.lang import *


x_hotel = ['book a hotel', 'need a nice place to stay', 'any motels near by']
x_weather = ['what is the weather like', 'is it hot outside']

clf = Classifier()
clf.fit(x_hotel, 'hotel')
clf.fit(x_weather, 'weather')

print(clf.predict('will it rain today'))  # >>> 'weather'
print(clf.predict('find a place to stay'))  # >>> 'hotel'
```

## Entity extractor

```python
from eywa.nlu import EntityExtractor

x = ['what is the weather in tokyo', 'what is the weather', 'what is the weather like in kochi']
y = [{'intent': 'weather', 'place': 'tokyo'}, {'intent': 'weather', 'place': 'here'}, {'intent': 'weather', 'place': 'kochi'}]

ex = EntityExtractor()
ex.fit(x, y)

x_test = 'what is the weather in london like'
print(ex.predict(x_test))
```

