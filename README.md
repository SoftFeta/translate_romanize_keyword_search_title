# translate_transliterate_keywords_search_title
Because I need it.

## Why?
Most news (whether they are fake or not) websites, for instance the supermajority of sites scraped from the [GDELT](https://www.gdeltproject.org/data.html) dataset, use **hypenated transliterated titles** as the URLs of their articles.

![image](https://user-images.githubusercontent.com/9071916/183798737-ef353816-e0f5-4153-907f-0f794c2c07ba.png)

## Functions
```
search(keyword, mandarin_on=False, japanese_on=False) -> (list of 4-tuples, set)
```
Enter a keyword in English and it will return a list of translated and transliterated keywords in small letters (spaces will become hyphens). It uses Wikipedia for translation instead of unreliable Google Translate.
The list of 4-tuples are the intermediate results for verification. The format of the 4-tuple is `(language_code, language_name, translated_but_not_transliterated, translated_and_transliterated)`. The set are the translated and transliterated results without repetition. The original keyword will be included in the set.

## Example
Take the keyword *quarantine* for example:
![image](https://user-images.githubusercontent.com/9071916/183834721-d06f5502-a75d-44ca-89e9-954ad585e635.png)

## Comparison with other libraries
* I have a very poor experience with the [`polyglot`](https://github.com/aboSamoor/polyglot) library because:
1. It requires cumbersome dependencies like PyICU, PyCLD2 (possibly needs Visual Studio on Windows as well) and Morfessor.
2. It requires downloading some corpi.
3. The library is no longer maintained.
4. For most Cyrillic transliterations (like Ukrainian and Russian), the transliteration schema is obsolete and weird, for example all 'y's are merged into 'i's.
5. It cannot handle special Cyrillic characters like in Kazakh and Kyrgyz.
6. It does not normalise accents. Basically it strips other diacritics or do nothing at all.
7. German transliteration makes no sense.
8. It does not work at all for systems not using Latin or Cyrillic characters. It simply returns an empty string or some weird consonants. (although it claims it does)

* [`transliterate`](https://github.com/barseghyanartur/transliterate)

This library does not work for languages that use the Latin script. It only works for Armenian, Georgian, Greek and some languages that uses the Cyrillic script. It does not support Kazakh and Kyrgyz, yet a considerable number of articles in the GDELT dataset are in Kazakh or Kyrgyz.

* [`google-transliteration-api`](https://github.com/NarVidhai/Google-Transliterate-API)

Does not transliterate into English at all.

* [`unidecode`](https://github.com/avian2/unidecode)

Not a transliteration library, it is only useful for stripping accents and diacritics. Does not handle German and Azerbaijani rules.

## German
*Ölpreis* ('oil price' in English) should be normalised to *oelpreis* for searching. Likewise, *Vorgefühl* to *vorgefuehl* and *nächst* to *naechst*. *weiß* should be *weiss*. All existing libraries do not handle these.

## Azerbaijani
[`unidecode`](https://github.com/avian2/unidecode) normalises the Azeri *ə* to *@* but news outlets use *e*.

## Mandarin (requires `mandarin_on=True`, default is `False`)
Hanyu pinyin is used.

## Japanese (requires `japanese_on=True`, default is `False`)
Rōmaji without macrons and extra vowels. So *rōmaji* becomes *romaji*.

## Not supported
Arabic, all Indic languages, all Dravidian languages, and all rare languages are not supported. This is because virtually no news outlets in these languages use transliteration in the URLs of articles.
