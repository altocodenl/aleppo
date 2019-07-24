# aleppo

> "Put another way, I started to believe that SDEs can be (or at least, should be) orders of magnitude more productive." -- [Steve Yegge](https://sites.google.com/site/steveyegge2/tin-foil-hats)

aleppo is an *uniform access hierarchical database*. It persists data on top of Node.js and Redis, and is a core part of [cells](https://github.com/altocodenl/cells).

aleppo is highly, highly experimental.

## Uniform access hierarchical database (uhdb)

An uhdb has four fundamental concepts:

- All data is indexed: all single queries should take about the same time.
- All data is stored in nested structures: all data is stored on a single object.
- There are four types of data: text, number, xlists (objects) and blists (arrays).
- Queries are code and data: queries can be expressed using the same data types; and queries are programs, executed one step at a time.

aleppo intends to have more querying power than a relational database with reasonable performance, while maintaining a hierarchical model (nestedness).

Note: we use the word *uniform access* instead of *random access* because the former is more accurate than the latter, despite not being standard.

### Everything indexed?

Having nested structures in databases (which is something that many NoSQL databases do) has been proven to be possible and practical. The idea of making all the data quickly accessible, however, is quite outrageous and it's still not clear it can be tenable. Indexes are expensive in space and time (processing).

Now, aleppo essentially stores numbers, text and pointers (which are also numbers). Nested structures (xlists and blists) can be represented by access paths which, expressed plainly, are merely a blist of texts and numbers. For example, if your root object is something like `[{name: 'aleppo', year: 2019}]` (the root object being in this case a blist), to access `2019`, the path would be `[0, 'year']`.

It is possible to use redis' sorted sets to keeping numbers and do all the required queries on them. The big challenge is how to efficiently store and query texts.

aleppo's core implementation idea is to use ngrams not just index but also store texts. Through the use of ngrams, it intends to piggyback compression onto the indexing, and then take advantage of the fact that most texts on a given database are going to look a lot like each other. Since the index is global, it is possible that the compression gains will be more than what could be expected from a compression algorithm using a sliding window.

For the moment, this is absolutely unproven. It is however the approach we're exploring.

### Operations

On text: equal, match (takes a regex).
On number: equal, greater, lesser, range.

## License

aleppo is written by [Altocode](https://altocode.nl) and released into the public domain.
