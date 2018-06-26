Title: Documentation

# Documentation

Welcome to `lingcorpora`'s documentation. In order to get started, see [Installation](#Installation) and then proceed to [Quickstart](#Quickstart). For parameter description, see [Making queries](#Making-queries). To obtain description of our `Target` and `Result` objects, go to [Working with results](#Working-with-results).

* [Installation](#Installation)
* [Quickstart](#Quickstart)
* [Making queries](#Making-queries)
  * [lingcorpora.Corpus](#lingcorpora.Corpus)
  * [Corpora](#Corpora)
* [Working with results](#Working-with-results)
  * [lingcorpora.Target](#lingcorpora.Target)
  * [lingcorpora.Result](#lingcorpora.Result)

## Installation

To install `lingcorpora`, type in the terminal:
```
pip install lingcorpora
```
lingcorpora has dependencies on:
* [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/)
* [lxml](http://lxml.de)
* [requests](http://docs.python-requests.org/en/master/)
* [tqdm](https://github.com/tqdm/tqdm)

`lingcorpora` will install these packages during installation. If you have troubles with installing dependencies, check Installation section in the documentation of the according packages.

## Quickstart

To search in any corpus, you need to create an instance of `Corpus` object and use method `search`:


```python
from lingcorpora import Corpus
rus_corp = Corpus('rus')
rus_results = rus_corp.search('мешок', numResults = 10)
rus_results
```

    "мешок": 100%|███████████████████████████████| 10/10 [00:07<00:00,  1.40docs/s]
    




    [Result(query=мешок, N=10, params={'numResults': 10, 'kwic': True, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'main', 'tag': False, 'start': 0, 'writingSystem': None})]



All results are stored in the `Result` object. `Result` is the list of `Target` objects. To print the data, you can use the following code:


```python
for result in rus_results:
    for i,target in enumerate(result):
        print(i+1,target.text)
```

    1  [lafet, nick]   «Считают, что если они кому-нибудь отсыпят мешок золота, то можно всех пригласить? 
    2  [Hamlet, nick]   ещё добрые преподы, которым просто некогда, и которые за мешок с пивом лихо ставили не самые плохие оценки всей группе ^ Но, но, но… 
    3  ))) И мешок для сменки Немо, и рюкзак, и пеналы-ручки-тетрадки. 
    4  Можешь использовать мешок многократно, простирывая после каждой варки с мылом, но только не в стиральном порошке. 
    5  И этот мешок ― как скатерть-самобранка; в нём всё прибывает орехов. 
    6  Наверняка пойдёт на дно, как мешок с песком!" 
    7   Капитан-шеф-повар побежал было на нос, чтобы увести Эле-Фантика Но как раз в этот миг налетел девятый вал и, подстегиваемый ураганом, рухнул, как девятиэтажный дом, точно на слонёнка - ослепил, оглушил, закрутил в водовороте и снёс за борт, как большой мешок с песком. 
    8   8. 39. Древнегреческий учёный Аристотель для доказательства невесомости воздуха взвешивал пустой кожаный мешок и тот же мешок, наполненный воздухом. 
    9   8. 39. Древнегреческий учёный Аристотель для доказательства невесомости воздуха взвешивал пустой кожаный мешок и тот же мешок, наполненный воздухом. 
    10   8. 39. Потому что вес мешка с воздухом увеличивался на столько, на сколько увеличивалась выталкивающая сила, действующая со стороны воздуха на раздутый мешок. 
    

Output can be transformed to `kwic` format:


```python
for result in rus_results:
    for i,target in enumerate(result):
        print(i+1,'\t'.join(target.kwic(left=5,right=5)))
```

    1 что если они кому-нибудь отсыпят	мешок	золота, то можно всех пригласить?
    2 просто некогда, и которые за	мешок	с пивом лихо ставили не
    3 ))) И	мешок	для сменки Немо, и рюкзак,
    4 Можешь использовать	мешок	многократно, простирывая после каждой варки
    5 И этот	мешок	― как скатерть-самобранка; в нём
    6 Наверняка пойдёт на дно, как	мешок	с песком!"
    7 снёс за борт, как большой	мешок	с песком.
    8 невесомости воздуха взвешивал пустой кожаный	мешок	и тот же мешок, наполненный
    9 кожаный мешок и тот же	мешок	,наполненный воздухом.
    10 со стороны воздуха на раздутый	мешок	.
    

You can search by multiple queries:


```python
rus_results = rus_corp.search(['сумка','мешок'], numResults = 10)
for result in rus_results:
    for i,target in enumerate(result):
        print(i+1,target.text)
    print()
```

    "сумка": 100%|███████████████████████████████| 10/10 [00:05<00:00,  1.81docs/s]
    "мешок": 100%|███████████████████████████████| 10/10 [00:06<00:00,  1.58docs/s]
    

    1  При этом у меня ещё была тяжеленная сумка и чужой дорогой фотоаппарат.. 
    2  И вдруг увидела, что её сумка разрезана. 
    3  Господин Эрмес-Дюма мягко намекнул Джейн, что её сумка для ручной клади ― эта была плетёная корзинка в стиле "воронье гнездо" ― выглядит как-то… ну, неподобающим для известной актрисы образом. 
    4  Простая на первый взгляд, классическая сумка стоит от $ 6 тысяч (хм, мог бы получиться неплохой автомобиль!), а чтобы её купить, нужно ждать от шести месяцев до двух лет, встав на waitlist! 
    5  За что сумка Birkin такая дорогая и отчего каждые девять секунд в мире кто-нибудь покупает платок-каре от Hermes? 
    6  Кроме того что эту сумочку обожала принцесса Грейс Келли и везде с ней появлялась, смотреть там особо не на что: простая классическая кожаная сумка, стоит при этом $ 12 тысяч (хм, мог бы получиться неплохой автомобиль!), а чтобы стать её обладательницей, нужно ждать минимум два года, встав на wait- list― и в очередь ещё не всех берут! 
    7   Новая сумка Dado из телячьей кожи ― несмотря на свои мягкие очертания, сумка не потеряет формы благодаря особой технологии 
    8   Новая сумка Dado из телячьей кожи ― несмотря на свои мягкие очертания, сумка не потеряет формы благодаря особой технологии 
    9  Спина у него была всего одна, и на ней уже громоздился вещмешок с толом и гранатами, плечо давила увесистая санитарная сумка, в руках он держал винтовку. 
    10  Санитарная сумка немного давила его плечо, но уж как-нибудь он её донесёт. 
    
    1  [lafet, nick]   «Считают, что если они кому-нибудь отсыпят мешок золота, то можно всех пригласить? 
    2  [Hamlet, nick]   ещё добрые преподы, которым просто некогда, и которые за мешок с пивом лихо ставили не самые плохие оценки всей группе ^ Но, но, но… 
    3  ))) И мешок для сменки Немо, и рюкзак, и пеналы-ручки-тетрадки. 
    4  Можешь использовать мешок многократно, простирывая после каждой варки с мылом, но только не в стиральном порошке. 
    5  И этот мешок ― как скатерть-самобранка; в нём всё прибывает орехов. 
    6  Наверняка пойдёт на дно, как мешок с песком!" 
    7   Капитан-шеф-повар побежал было на нос, чтобы увести Эле-Фантика Но как раз в этот миг налетел девятый вал и, подстегиваемый ураганом, рухнул, как девятиэтажный дом, точно на слонёнка - ослепил, оглушил, закрутил в водовороте и снёс за борт, как большой мешок с песком. 
    8   8. 39. Древнегреческий учёный Аристотель для доказательства невесомости воздуха взвешивал пустой кожаный мешок и тот же мешок, наполненный воздухом. 
    9   8. 39. Древнегреческий учёный Аристотель для доказательства невесомости воздуха взвешивал пустой кожаный мешок и тот же мешок, наполненный воздухом. 
    10   8. 39. Потому что вес мешка с воздухом увеличивался на столько, на сколько увеличивалась выталкивающая сила, действующая со стороны воздуха на раздутый мешок. 
    
    

[To the top](#Documentation)

## Making queries
 

### lingcorpora.Corpus

lingcorpora.Corpus(language, sleep_time=1, sleep_each=5)<br>
The object of this class should be instantiated for each corpus. Search is conducted via `search` method.

#### Attributes
* **language**: str: in most cases, language [ISO 639-3 code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) for the corpus with combined codes for parallel corpora. List of available corpora with corresponding codes:

| Code         | Corpus                                                                        |
|--------------|-------------------------------------------------------------------------------|
| bam          | [Corpus Bambara de Reference](#Corpus-Bambara-de-Reference)                   |
| emk          | [Maninka Automatically Parsed corpus](#Maninka-Automatically-Parsed-corpus)   |
| rus          | [National Corpus of Russian Language](#National-Corpus-of-Russian-Language)   |
| rus_parallel | [Parallel subcorpus of National Corpus of Russian Language](#Parallel-subcorpus-of-National-Corpus-of-Russian-Language) |
| zho          | [Center of Chinese Linguistics corpus](#Center-of-Chinese-Linguistics-corpus) |

* **sleep_time**: int, optional, default 1: the length of pause between requests to the corpus (in seconds). It is required to avoid blocking and corpus breakdown.
* **sleep_each**: int, optional, default 5: the number of requests after which a pause is required.
* **doc**: str: documentation for chosen corpus (after instance creation).
* **results**: list: list of all `Result` objects, each returned by `search` method.
* **failed**: list: list of `Result` objects where nothing was found.

#### lingcorpora.Corpus.search(query, \*args, \*\*kwargs)

This is a search function that queries the corpus and returns the results.

##### Arguments

Here we provide a comprehensive list of arguments, although not all arguments are applicable to all corpora and the default values depend on a corpus. For specific description, go to [Corpora](#Corpora) subsection.

Two arguments are universal:

* **query**: str or list[str]: query or queries
* **numResults**: int, optional, default 100: number of results wanted

Other depend on the corpus:

* **kwic**: boolean, optional, default True: `kwic` format (True) or a sentence (False)
* **nLeft**: int, optional: number of words / symbols (corpus-specific) in the left context
* **nRight**: int, optional: number of words / symbols (corpus-specific) in the right context
* **queryLanguage**: str: for parallel corpora, language of the query
* **subcorpus**: str, optional: subcorpus to search in
* **tag**: boolean, optional, default False: whether to download grammatical information if the corpus is annotated
* **writingSystem**: str, optional: writing system of results
 

##### Output

* **Result**: list: resuls for queries in current search

`search` returns the result of current query and adds it to `results` attribute of corresponding `Corpus` object. In the following example, `first_result` and `second_result` contain results of two queries (the second consists of two `Result` objects), whereas `bam_corp.results` contains all results obtained from the corpus.


```python
bam_corp = Corpus('bam')

first_result = bam_corp.search('jamana', numResults = 10)
print(first_result)
second_result = bam_corp.search(['kuma','fɔra'], numResults = 10)
print(second_result)

bam_corp.results
```

    "jamana": 100%|██████████████████████████████| 10/10 [00:02<00:00,  4.23docs/s]
    

    [Result(query=jamana, N=10, params={'numResults': 10, 'kwic': True, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'corbama-net-non-tonal', 'tag': False, 'start': 0, 'writingSystem': None})]
    

    "kuma": 100%|████████████████████████████████| 10/10 [00:02<00:00,  4.36docs/s]
    "fɔra": 100%|████████████████████████████████| 10/10 [00:02<00:00,  4.42docs/s]
    

    [Result(query=kuma, N=10, params={'numResults': 10, 'kwic': True, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'corbama-net-non-tonal', 'tag': False, 'start': 0, 'writingSystem': None}), Result(query=fɔra, N=10, params={'numResults': 10, 'kwic': True, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'corbama-net-non-tonal', 'tag': False, 'start': 0, 'writingSystem': None})]
    




    [Result(query=jamana, N=10, params={'numResults': 10, 'kwic': True, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'corbama-net-non-tonal', 'tag': False, 'start': 0, 'writingSystem': None}),
     Result(query=kuma, N=10, params={'numResults': 10, 'kwic': True, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'corbama-net-non-tonal', 'tag': False, 'start': 0, 'writingSystem': None}),
     Result(query=fɔra, N=10, params={'numResults': 10, 'kwic': True, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'corbama-net-non-tonal', 'tag': False, 'start': 0, 'writingSystem': None})]



#### lingcorpora.Corpus.retry_failed()

This function redoes queries that returned empty results.

##### Arguments
There are no arguments, failed queries are taken from `failed` attribute of the `Corpus` instance.

##### Output
List of `Result` objects for which the second try was successful. It also appends successful results to the `results` attribute of the `Corpus` instance and rewrites `failed` attribute with queries that remained unsuccessful.

[To the top](#Documentation)

### Corpora

#### [Corpus Bambara de Reference](http://maslinsky.spb.ru/bonito/run.cgi/first_form)
##### Relevant parameters
* **query**: str or list[str]: query or queries (currently only exact search by word or phrase is available)
* **numResults**: int, default 100: number of results wanted
* **kwic**: boolean, default True: `kwic` format (True) or a sentence (False)
* **tag**: boolean, default False: whether to collect grammatical tags for target word or not (available only for `corbama-net-non-tonal` subcorpus)
* **subcorpus**: str, default `corbama-net-non-tonal`: subcorpus. Available options: `corbama-net-non-tonal`, `corbama-net-tonal`, `corbama-brut`

##### Example of use


```python
corp = Corpus('bam')
results = corp.search('kɔ́nɔ', numResults = 10, subcorpus='corbama-net-tonal')
for result in results:
    for i,target in enumerate(result):
        print(i+1,target.text)
```

    "kɔ́nɔ": 100%|███████████████████████████████| 10/10 [00:02<00:00,  4.46docs/s]
    

    1 à fɔra kó mɔ̀gɔ sí kànâ táa kúngo kɔ́nɔ sélidonya fɛ̀ . àlê y' í túgu kà
    2 y' í túgu kà táa . à sélen kúngo kɔ́nɔ , à y' à sɔ̀rɔ warabilenw bɛ́ tulonkɛ
    3 fɔɲi-fɔɲi » . npogotiginin seginna dùgu kɔ́nɔ , k' ò ɲɛ́fɔ à bá yé . kàbini
    4 kà kɛ́ wárabilen yé , kà táa kúngo kɔ́nɔ . ò kɔ́ , dùgu denmisɛnw bɛ́ɛ ye
    5 kà kɛ́ warabilenw yé , kà táa kúngo kɔ́nɔ . Dugumɔgɔw bɛ́ɛ yɛlɛmana kà kɛ́ warabilenw
    6 dá . jùla dɔw tɛ̀mɛntɔ dùgu ìn kɔ́nɔ , ù kabakoyara kà mùsokɔrɔba ìn kélen
    7 mùsokɔrɔba kó , kó dénnin dɔ́ taara kúngo kɔ́nɔ sélidonya fɛ̀ . à ye warabilenw sɔ̀rɔ
    8 lá . kàbini dénnin ìn seginna dùgu kɔ́nɔ kà sègin dɔ̀nkili ìn kàn à bá yé
    9 yɛlɛmana kà kɛ́ wárabilen yé kà táa kúngo kɔ́nɔ . ò kɔ́ , dùgu mɔgɔw ye dɔ̀nkili ìn
    10 yɛlɛmana kà kɛ́ wárabilen yé kà táa kúngo kɔ́nɔ . ǹka , nê ma sɔ̀n kà à dá , ò dè
    

[To the top](#Documentation)

#### [Maninka Automatically Parsed corpus](http://maslinsky.spb.ru/emk/run.cgi/first_form)
##### Relevant parameters
* **query**: str or list[str]: query or queries (currently only exact search by word or phrase is available)
* **numResults**: int, default 100: number of results wanted
* **kwic**: boolean, default True: `kwic` format (True) or a sentence (False)
* **subcorpus**: str, default `cormani-brut-lat`: subcorpus. Available options: `cormani-brut-lat`, `corbama-brut-nko`
* **writingSystem**: str, default `None`: writing system for examples. Available options: `nko`, `latin`. Bug: only `latin` for `corbama-brut-nko` subcorpus.

##### Example of use


```python
corp = Corpus('emk')
results = corp.search('tuma bɛɛ', numResults = 10, writingSystem='nko', kwic=False)
for result in results:
    for i,target in enumerate(result):
        print(i+1,target.text)
```

    "tuma bɛɛ": 100%|████████████████████████████| 10/10 [00:02<00:00,  4.42docs/s]
    

    1 ߁߄ ߊߟߎ ߕߍߘߍ ߊߟߎ ߟߊߘߍߟߊ ߊߟߟߊ ߕߊߙߊ ߞߊ߲ߡߊ ߕߎߡߊ ߓߍ߯ .
    2 ‹ ߣ ߞߊ ߝߊ߯ߡߊ ߦߋ ߣ ߢߍ ߕߎߡߊ ߓߍ߯ .
    3 ߊߟߎ ߞߊ߲ ߞߏ : « ߗߍ ߣߌ߲ ߦߋ ߞߎߡߊ ߖߎ߯ ߝߐߟߊ ߦߐߙߐ ߛߊߣߌߡߊ߲ ߣߌ߲ ߡߊ ߕߎߡߊ ߓߍ߯ , ߞߊ ߞߎߡߊ ߖߎ߯ ߝߐ ߞߋߟߊ ߡߎߛߊ ߟߊ ߛߊߙߌߦߊ ߝߊߣߊ߲ ߡߊ .
    4 ߁ ߥߏ ߕߎߡߊ , ߛߐߟߌ ߕߍߘߍ ߝߊ߯ߡߊ ߌߛߊ ߟߊ ߞߊߙߊ߲ߘߋ߲ߣߎ ߟߊ ߢߊߣߊߡߊߦߊ ߡߊߛߌߟߊ߲ߣߊ ߕߎߡߊ ߓߍ߯ .
    5 ߡߏߛߏ ߥߏ ߕߍߘߍ ߞߏߢߎߡߊ ߞߍߟߊ ߕߎߡߊ ߓߍ߯ ߞߊ ߝߊ߲ߕߊ߲ߣߎ ߘߍߡߍ߲ .
    6 ߊ ߕߍߘߍ ߦߊߤߎߘߌߦߊ ߝߊ߲ߕߊ߲ߣߎ ߛߐߟߊ ߞߏߛߍߓߍ , ߞߊ ߊߟߟߊ ߕߊߙߊ ߕߎߡߊ ߓߍ߯ .
    7 ߁߇ ߞߐߣߌ߲ ߊ ߕߍߘߍ ߊ ߖߍߘߍ ߦߌߘߊߞߊߟߊ ߊߟߎ ߟߊ ߕߎߡߊ ߓߍ߯ ߊߟߊ ߞߋߥߊߟߌߢߎߡߊߟߎ ߝߍ .
    8 ߁߆ ߥߏ ߟߋ ߘߐ , ߣ ߦߋ ߣ ߘߐߖߊߟߊ ߕߎߡߊ ߓߍ߯ ߛߊ ߣ ߞߐߣߐߜߍߦߊߣߍ߲ ߘߌ ߕߏ .
    9 ߥߏ ߟߋ ߘߐ , ߊ ߕߍߘߍ ߔߐߟߌ ߞߌߟߌߟߊ ߕߎߡߊ ߓߍ߯ ߞߊ ߓߊߘߏ ߞߍ ߊ ߝߍ .
    10 ߂߉ ߣ ߦߋ ߊ ߝߍ ߟߋ ߖߎߛߎ ߥߏ ߢߐ߲߰ ߦߋ ߞߍ ߊߟߎ ߓߏߟߏ ߕߎߡߊ ߓߍ߯ ߞߊ ߛߌߟߊ߲ ߣ ߢߍ ߞߊ ߣߣߊ ߖߊߡߊߙߌߟߌߟߎ ߓߍ߯ ߟߊߕߋߟߋ߲ , ߛߊ ߊߟߎ ߣߌ ߊߟߎ ߞߐߡߐ߰ߟߎ ߓߍ߯ ߘߌ ߤߍߙߍ ߛߐߘߐ߲ ߞߊߘߊߥߎ .
    

[To the top](#Documentation)

#### [National Corpus of Russian Language](http://ruscorpora.ru)
##### Relevant parameters
* **query**: str or list[str]: query or queries (currently only exact search by word or phrase is available)
* **numResults**: int, default 100: number of results wanted
* **kwic**: boolean, default True: `kwic` format (True) or a sentence (False)
* **tag**: boolean, default False: whether to collect grammatical tags for target word or not
* **subcorpus**: str, default `main`: subcorpus. Available options:`main`, `syntax`, `paper`, `regional`, `school`, `dialect`, `poetic`, `spoken`, `accent`, `murco`, `multiparc`, `old_rus`, `birchbark`, `mid_rus`, `orthlib`.

##### Example of use


```python
corp = Corpus('rus')
results = corp.search('сердце', numResults = 10, subcorpus='poetic')
for result in results:
    for i,target in enumerate(result):
        print(i+1,target.text)
```

    "сердце": 100%|██████████████████████████████| 10/10 [00:04<00:00,  2.22docs/s]
    

    1   Не будильник поставлен на шесть,  а колотится сердце быстрей. 
    2   Да и сердце легче бьется ― поддается уговорам. 
    3   Прелесть, от нее дрожит сердце возле птиц или ночниц бледных. 
    4   Чтоб они стали перинами белыми с мягкой опорой на дне,  и невредимыми съехали, целыми дети на той стороне.   Сердце привязано ниткой невидимой.   Нить коротка, а земля велика. 
    5   Моя мама умерла девятого мая, когда всюду день-деньской надрывают сердце «аты-баты» ― коллективный катарсис такой. 
    6  Чтобы скорей, скорей горло его достать.   Сердце его потрогать. 
    7   И ― прочь через площадь в закатных лучах В какой-нибудь Чехии, Польше… Разбитое сердце, своя голова на плечах ― Чего тебе больше? 
    8   «Хотел бы я знать, если Бог повелит,  О чем твое старое сердце болит». 
    9   Когда уйдет последний друг И в сердце перемрут подруги,  Я очерчу незримый круг И лиру заключу в том круге. 
    10   О том не надо вспоминать,  Но что-то в сердце изломилось:  ― Не узнаю родную мать. 
    

[To the top](#Documentation)

#### [Parallel subcorpus of National Corpus of Russian Language](http://ruscorpora.ru/search-para-multi.html)
##### Relevant parameters
* **query**: str or list[str]: query or queries (currently only exact search by word or phrase is available)
* **queryLanguage**: str: language of the query
* **numResults**: int, default 100: number of results wanted
* **kwic**: boolean, default True: `kwic` format (True) or a sentence (False)
* **tag**: boolean, default False: whether to collect grammatical tags for target word or not
* **subcorpus**: str, default `rus` (search by all target languages): subcorpus. Available options: `rus`, `eng`, `bel`, `bul`, `bua`, `esp`, `ita`,`zho`, `lav`, `ger`, `pol`, `ukr`, `fra`, `sve`, `est`.

##### Example of use


```python
corp = Corpus('rus_parallel')
results = corp.search('авось', queryLanguage='rus', numResults = 10)
for result in results:
    for i,target in enumerate(result):
        print(i+1,target.transl.strip(),'\n',target.text.strip())
```

    "авось": 100%|███████████████████████████████| 10/10 [00:03<00:00,  3.04docs/s]
    

    1 — Un Russe, ça tient sur trois béquilles: “on sait jamais”, “on verra bien”, et “on s'en sortira toujours”. 
     – Русский человек крепок на трех сваях – «авось», «небось» и «как-нибудь».
    2 – Да ви кажа, не ми се вярва… – Порфирий Петрович махна с ръка. – И нищо няма да излезе от цялата работа Съвсем безнадежден случай… Но впрочем идете.    Може да извадите по-голям късмет от мен.    И позна. 
     – Я признаться уж не верю-с… – Порфирий Петрович махнул рукой. – Ничего мы тут не зацепим. Кругом одна безнадежность… А впрочем сходите.    Авось вам больше моего повезет.    И ведь как в воду смотрел.
    3 Руският злодей е пламенен и недалновиден, убива както дойде. 
     Русский злодей горяч и нерасчетлив, крушит на авось.
    4 Първо, престъплението се оказа все пак не европейско, а руско, стремглаво. 
     Во-первых, преступление все-таки оказалось не европейское, а русское, на авось.
    5 И сложете на всекиго номер.    Да се надяваме, че няма да ги объркаме.    Вече ги научих всички наизуст. 
     Да еще нумер каждому проставьте.    Ничего, авось не перепутаем-с.    Я их всех уж наизусть успел выучить.
    6 Nikt, rzecz jasna, nie zabroni waszej dostojności słać dalszych epistoł, kropla wszak drąży skałę, kto wie, może któryś z kardynałów wreszcie ulegnie, może wreszcie mnie odwołają? 
     Никто, естественно, не запретит вашей милости кропать новые эпистолы, ибо капля камень точит, как знать, авось кто-нибудь из кардиналов наконец поддастся, возможно, меня наконец отзовут.
    7 Становішча яго, канешне, было зусім ахавае, хаця ў глыбіні сьвядомасьці ўсё ж таілася маленькая спадзёўка.    На авось ці, можа, на цуд.    Калі іншага не было, звычайна спадзяваліся на цуд, мабыць, так было заўжды. 
     Положение его, конечно, аховое, но в глубине сознания все-таки теплилась маленькая надежда.    На авось или, может, на чудо.    Когда другого не находилось, обычно полагались на чудо, так было всегда.
    8 Становішча яго, канешне, было зусім ахавае, хаця ў глыбіні сьвядомасьці ўсё ж таілася маленькая спадзёўка.    На авось ці, можа, на цуд.    Калі іншага не было, звычайна спадзяваліся на цуд, мабыць, так было заўжды. 
     Положение его, конечно, аховое, но в глубине сознания все-таки теплилась маленькая надежда.    На авось или, может, на чудо.    Когда другого не находилось, обычно полагались на чудо, так было всегда.
    9 Цуд, вядома, памагаў рэдка, болей падводзіў, асабліва атэістаў-бальшавікоў, якія, быццам не ведаючы таго, цяпер так дружна рушылі да хрысьціянскіх цудаў.    Авось паможа!    Калі памаліцца Богу, абразам, паклікаць царкоўных іерархаў на шматлюдныя прапагандовыя шоу. 
     Чудо, конечно, выручало редко, больше подводило, особенно атеистов-большевиков, которые теперь так дружно стали взывать к христианскому чуду.    Авось выручит!    ―
    10 Цуд, вядома, памагаў рэдка, болей падводзіў, асабліва атэістаў-бальшавікоў, якія, быццам не ведаючы таго, цяпер так дружна рушылі да хрысьціянскіх цудаў.    Авось паможа!    Калі памаліцца Богу, абразам, паклікаць царкоўных іерархаў на шматлюдныя прапагандовыя шоу. 
     Чудо, конечно, выручало редко, больше подводило, особенно атеистов-большевиков, которые теперь так дружно стали взывать к христианскому чуду.    Авось выручит!    ―
    

[To the top](#Documentation)

#### [Center of Chinese Linguistics corpus](http://ccl.pku.edu.cn:8080/ccl_corpus/index.jsp) 
##### Relevant parameters
* **query**: str or list[str]: query or queries (currently only exact search by word or phrase is available)
* **numResults**: int, default 100: number of results wanted
* **subcorpus**: str, default `xiandai`: subcorpus. Available options: `xiandai` (modern Chinese) or `dugai` (ancient Chinese).
* **nLeft**, **nRight**: int, default 30: context lenght (in symbols)

##### Example of use


```python
corp = Corpus('zho')
results = corp.search('能力#3大', numResults = 10, nLeft=15, nRight=15)
for result in results:
    for i,target in enumerate(result):
        print(i+1,target.text)
```

    "能力#3大": 100%|███████████████████████████████| 10/10 [00:04<00:00,  2.10docs/s]
    

    1 ...的人生方向。每年韩国高考--"大学修业能力考试"举行当日，所有的公务员和私...
    2 ...学生的品德如何、修养怎样、生存能力大小、自立能力强弱等这些关系孩子生...
    3 ...索引擎从网络中获取信息的动力和能力；大学前的教育大多是在以教师为中心、...
    4 ...的信息化社会中，处理非确定信息能力的大小将会成为衡量一个教师作用大小的...
    5 ...，法国中、小学生识字和拼写法文能力大幅度下降。在一项对15岁学生法文...
    6 ...公司的股东能决定要如何使用生产能力。在更大的公司里，公司的权力架构通常有一...
    7 ...如某些弹性纤维就具有弹力(变形能力)大、屈光好、韧度高以及抗酸碱侵蚀性...
    8 ...缩小,导致国家对整个社会的控制能力大大降低。例如，在SARS防治需要...
    9 ...六，七岁）。幼儿的再认识和回忆能力有很大发展，回忆范围逐渐扩大了。其特点...
    10 ...来，拍起马来，比哪一个国家的人能力都大。因之这里所谓“私”的问题却是个...
    

[To the top](#Documentation)

## Working with results

### lingcorpora.Result

The object of this class contains all results found. `Result` object is iterable and supports indexing.

#### Attributes

* **results**: list[`Target`]: list of results
* **N**: int: number of results
* **lang**: str: corpus language
* **query**: str: search query
* **params**: dict: all other parameters of the search


```python
corp = Corpus('emk')
results = corp.search('tuma', numResults = 10, kwic=False)[0]
results
```

    "tuma": 100%|████████████████████████████████| 10/10 [00:02<00:00,  4.43docs/s]
    




    Result(query=tuma, N=10, params={'numResults': 10, 'kwic': False, 'nLeft': None, 'nRight': None, 'queryLanguage': None, 'subcorpus': 'cormani-brut-lat', 'tag': False, 'start': 0, 'writingSystem': ''})



#### lingcorpora.Result.export_csv(filename=None, header=True, sep=';')

Writes results into a `csv` file considering `kwic` parameter of the search.

##### Arguments
* **filename**: str, optional, default `None`: name of the file. If `None`, filename is *lang*_*query*_results.csv with omission of disallowed filename symbols
* **header**: boolean, optional, default True: whether to include a header in the table
* **sep**: str, optional, default `;`: cell separator in the csv

##### Output
None, writes results into the file.

#### lingcorpora.Result.clear()

Overwrites the `results` attribute to empty list.


```python
print(results.results)
results.clear()
print(results.results)
```

    [Target( tuma, ), Target( tuma, ), Target( tuma, ), Target( tuma, ), Target( tuma, ), Target( tuma, ), Target( tuma, ), Target( tuma, ), Target( tuma, ), Target( tuma, )]
    []
    

[To the top](#Documentation)

### lingcorpora.Target

`Target` contains one item from the result list.

#### Attributes
* **text**: str: full example (sentence or `kwic`)
* **idxs**: idxs: tuple (l, r): target indices in **text** -> text[l:r]
* **meta**: str: sentence / document metadata, e.g. document name, author, etc. (if exists)
* **tags**: dict: target tags
* **transl**: str: text translation (only for parallel corpora)
* **lang**: str: translation language (only for parallel corpora)


```python
rus_corp = Corpus('rus')
rus_results = rus_corp.search('одеяло', numResults = 10, tag=True)[0]
first_hit = rus_results[0]
first_hit
```

    "одеяло": 100%|██████████████████████████████| 10/10 [00:08<00:00,  1.18docs/s]
    




    Target(одеяло, Народный костюм: архаика или современность? // «Народное творчество», 2004)




```python
for k,v in vars(first_hit).items():
    print(k,v)
```

    text  Я, например, для внучки настегала своими руками лоскутное одеяло, зная, что оно будет её оберегать, давать ей энергию. 
    idxs (59, 65)
    meta Народный костюм: архаика или современность? // «Народное творчество», 2004
    tags {'lex': ['одеяло'], 'gramm': ['S', 'inan', 'n', 'sg', 'acc', 'disamb'], 'sem': ['r:concr', 't:tool:bedding'], 'flags': ['animred', 'bcomma', 'bmark', 'casered', 'genderred', 'numred']}
    transl None
    lang None
    

#### lingcorpora.Target.kwic(left, right, level='word')

This function makes `kwic` format for an item for further usage and csv output.

##### Attributes
* **left**: int: length of left context
* **right**: int: length of right context
* **level**: str, optional, default `word`: counting context length by tokens (`word`) or by characters (`char`)

##### Output
tuple: (left context, target, right context)


```python
print(first_hit.kwic(left=5,right=5))
print(first_hit.kwic(left=30,right=30, level='char'))
```

    ('внучки настегала своими руками лоскутное', 'одеяло', ',зная, что оно будет её')
    ('егала своими руками лоскутное ', 'одеяло', ', зная, что оно будет её обере')
    

[To the top](#Documentation)
