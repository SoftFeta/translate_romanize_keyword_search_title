# transliterate_keywords_search_title
Because I need it.

# Why?
Most news (whether they are fake or not) websites, for instance the supermajority of sites scraped from the GDELT dataset, use **hypenated transliterated titles** as the URLs of their articles.

# Functions
```
search(keyword) -> (list of 3-tuples, set)
```
Enter a keyword in English and it will return a list of translated and transliterated keywords in small letters (spaces will become hyphens). It uses Wikipedia for translation instead of unreliable Google Translate.
The list of 3-tuples are the intermediate results for verification. The format of the 3-tuple is `(language_code, translated_but_not_transliterated, translated_and_transliterated)`. The set are the translated and transliterated results without repetition. The original keyword will be included in the set.

# Comparison with other libraries
* I have a very poor experience with the `polyglot` library because:
1. It requires cumbersome dependencies like PyICU, PyCLD2 (possibly needs Visual Studio on Windows as well) and Morfessor.
2. The library is no longer maintained.
3. For most Cyrillic transliterations (like Ukrainian and Russian), the transliteration schema is obsolete and weird, for example all 'y's are merged into 'i's.
4. It cannot handle special Cyrillic characters like in Kazakh and Kyrgyz.
5. It does not normalise accents. Basically it strips other diacritics or do nothing at all.
6. German transliteration makes no sense.
7. It does not work at all for systems not using Latin or Cyrillic characters. It simply returns an empty string or some weird consonants. (although it claims it does)

* `transliteration`

It does not work for languages that use the latin script. It only works for Armenian, Georgian, Greek and some languages that uses the Cyrillic script. It does not support Kazakh and Kyrgyz, yet a considerable number of articles in the GDELT dataset are in Kazakh or Kyrgyz.

* `google-transliteration`

Does not transliterate into English at all.

# German
*Ölpreis* ('oil price' in English) should be normalised to *oelpreis* for searching. Likewise, *Vorgefühl* to *vorgefuehl* and *nächst* to *naechst*. *weiß* should be *weiss*. All existing libraries do not handle these.

# Azerbaijani
`unidecode` normalises the Azeri *ə* to *@* but news outlets use *e*.

# Japanese
Rōmaji without macrons and extra vowels. So *rōmaji* becomes *romaji*.

# Mandarin
Hanyu pinyin is used.

# Not supported
Arabic, all Indic languages, all Dravidian languages, and all rare languages are not supported.
