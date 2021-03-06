---
title: Searching
number: 2
---
# Searching

After you have built an index of your documents, the next step is to perform a search.

The simplest way to start is to pass the text on which you want to search into the search method:

```javascript
idx.search('foo')
```

The above will return details of all documents that match the term "foo". Although it looks like a string, the `search` method parses the string into a search query. This supports special syntax for defining more complex queries.

Searches for multiple terms are also supported. If a document matches _at least_ one of the search terms, it will show in the results. The search terms are combined with OR.

```javascript
idx.search('foo bar')
```

The above example will match documents that contain either "foo" or "bar". Documents that contain _both_ will score more highly and will be returned first.

## Wildcards

Lunr supports wildcards when performing searches. A wildcard is represented as an asterisk (`*`) and can appear anywhere in a search term. For example, the following will match all documents with words beginning with "foo":

```javascript
idx.search('foo*')
```

This will match all documents that end with 'oo':

```javascript
idx.search('*oo')
```

Leading wildcards, as in the above example, should be used sparingly. They can have a negative impact on the performance of a search, especially in large indexes.

Finally, a wildcard can be in the middle of a term. The following will match any documents that contain a term that begins with "f" and ends in "o":

```javascript
idx.search('f*o')
```

It is also worth noting that, when a search term contains a wildcard, no stemming is performed on the search term.

## Fields

By default, Lunr will search all fields in a document for the query term, and it is possible to restrict a term to a specific field. The following example searches for the term "foo" in the field title:

```javascript
idx.search('title:foo')
```

The search term is prefixed with the name of the field, followed by a colon (`:`). The field _must_ be one of the fields defined when building the index. Unrecognised fields will lead to an error.

Field-based searches can be combined with all other term modifiers and wildcards, as well as other terms. For example, to search for words beginning with "foo" in the title or with "bar" in any field the following query can be used:

```javascript
idx.search('title:foo* bar')
```

## Boosts

In multi-term searches, a single term may be important than others. For these cases Lunr supports term level boosts. Any document that matches a boosted term will get a higher relevance score, and appear higher up in the results. A boost is applied by appending a caret (`^`) and then a positive integer to a term.

```javascript
idx.search('foo^10 bar')
```

The above example weights the term "foo" 10 times higher than the term "bar". The boost value can be any positive integer, and different terms can have different boosts:

```javascript
idx.search('foo^10 bar^5 baz')
```

## Fuzzy Matches

Lunr supports fuzzy matching search terms in documents, which can be helpful if the spelling of a term is unclear, or to increase the number of search results that are returned. The amount of fuzziness to allow when searching can also be controlled. Fuzziness is applied by appending a tilde (`~`) and then a positive integer to a term. The following search matches all documents that have a word within 1 edit distance of "foo":

```javascript
idx.search('foo~1')
```

An edit distance of 1 allows words to match if either adding, removing, changing or transposing a character in the word would lead to a match. For example "boo" requires a single edit (replacing "f" with "b") and would match, but "boot" would not as it also requires an additional "t" at the end.
