# Making new API

## Main concept

* Search is done via `Corpus` object (`search` method)
* Each API of a corpus, dictionary, etc. is a `PageParser` class (see below) which has method `.extract()`.
* `PageParser.extract()` is a generator (see `yield` in Python) of `Target` objects (individual hits).
* `PageParser` inherits from `Container`, which is a class in `params_container.py` and contains all possible parameters for corpora.
* All `Target` objects are collected in `search` (in the `Corpus` object) into the `Result` object.
* Documentation for users can be found [here](https://lingcorpora.github.io/lingcorpora.py/docs.html)

## To make a new API
1. Make a `PageParser` object
  1. It inherits from `Container` and `Container` constructor is called in `__init__` (see example below)
  2. It has method `extract()` which `yield`s `Target` objects
  3. All other (auxiliary) parameters in `PageParser` should be encapsulated (add to underscores `__` to their names)
2. You should pass to `Target` object the following information:
  1. whole sentence (`text`) - string
  2. indices (`idxs`) of the target in the sentence: `l` and `r` such that target == `text[l:r]` - tuple
  3. metadata (`meta`) (document name, author, year, etc.) - string. If there is no meta, then pass empty string
  4. grammar tags (`tags`) - dict. If there are no tags, pass empty dict
  5. for parallel corpora: translation (`transl`) - translation from `queryLanguage` to another language
  6. for parallel corpora: language (`lang`) - the other language (not `queryLanguage`) in the example pair
  7. **Important**: if there are several target occurrences in one example, you should split them into **separate** Target objects.
3. Write the docstring `__doc__` and the author `__author__` before `PageParser`
4. Name the file *langcode*\_corpus.py and place it into the `corpora` directory. *langcode* stands for [ISO 639-3 code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
5. For testing purposes the following functions must be provided (see implementation details in the template below):
    1. `test_single_query`: single query processing test-run
    2. `test_num_results`: assert that amount of data extracted matches the passed one via `numResults` arg
    3. `test_multi_query`: provide `query` arg for the multiple query
6. If you would like to add new search parameters, open `params_container.py` and add this parameter to the arguments (**do not forget default value**) and attributes.
7. Make a pull request and if API is OK, we will:
  1. Add it to the package
  2. Include it in the docs
  
## API template


```python
from params_container import Container
from target import Target


__author__ = ''
__doc__ = \
"""
"""


class PageParser(Container):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        # inner auxiliary attributes:
        # self.__page = None
        # self.__pagenum = 0
        # ...
    
    def any_method_for_getting_the_results(self):
        pass
    
    # ...
    
    def any_method_for_getting_the_results_10(self):
        pass
    
    def extract(self):
        """
        --- Generator of found occurrences as `Target` types
            Query.search() uses this method---
        """
        # ...
        
        # for each occurrence found we pass `Target` object,
        # describing the occurrence, to Query.search()
        # for parallel corpora also transl and lang
        for text, idxs, meta, tags in found:
            yield Target(text, idxs, meta, tags)


def test_single_query():
    """
    *This is a possible valid template*
    
    process single query passed as `query` arg
    and make sure that all the Targets found
    match the `query`
    """
    
    query = <str>

    parser = PageParser(query=query,
                        numResults=5
                        )        

    for t in parser.extract():
        l, r = t.idxs
        assert t.text[l:r] == query, \
               'does not match `query`: `%s`' % t.text[l:r]

    del parser
    
    
def test_num_results():
    """
    *This is a possible valid template*
    
    assert that the amount of Targets found
    matches the one passed via `numResults` arg
    """
    
    numResults = <int>
    query = <str>
    
    parser = PageParser(query=query,
                        numResults=numResults
                        )        
    
    res = list(parser.extract())

    assert len(res) == numResults    

    del parser, res
    
    
def test_multi_query():
    """
    provide `query` arg as iterable[<str>]
    for low-level testing of multiple query processing
    """
    
    return [<str1>, <str2>]
```
