---
title: "CarashGolangES"
date: 2018-05-17T05:43:14+07:00
author: Wibisana Bramawidya
tags: ["Wibisana", "Golang", "ES", "Arango"]
---

> I shouldn't even be here. What am I doing here. Oh god everything has gone wrong.

## Apologies

First off, I would like to apologize for the lack of posts these last 2 weeks. For the past 2 weeks, I seem to be out of it, just out of everything. I have been missing classes, assignments, deadlines, and as you can probably see, even this project. This past month has been my least productive month, probably of my whole college experience. I hate to say it, but my contributions to this project has been dwindling, and I may not be able to contribute much anymore.

## Progress

Well, at least even though my contribution are close to nothing, they are not **COMPLETELY** nothing. In the last month, I have at least made progress in 1 aspect, thank god. The main focus of my last month was trying to make the Elastic Search for searching the products work. All the while encountering various issues unrelated to the application itself, like the server being too weak and such. Either way, in the end I made few progress, but progress nonetheless.

#### Arango

In the last several weeks, trying to wrestle with ArangoDB's query language has been a hurdle. Using AQL instead of the usual SQL is pretty confusing. Just look at this query.

```
FOR product IN Product
    FOR merchant IN 0..1 INBOUND product Owns
        FILTER merchant._id LIKE 'Merchant/%'
        COLLECT check = true INTO group
    RETURN group
```

Just, *WHAT IS THIS*. What's more, this query returns a nested array of the result

```Json
[
  [
    {
      "product": {...},
      "merchant": {...}
    },
    {
      "product": {...},
      "merchant": {...}
    },
    ...
  ]
]
```

That alone is frustrating enough, not including the time spent just learning the **damn** thing. Though I must say, processing data from an Arango query is much easier than any SQL supporting DB out there. Since it returns a JSON object, its really easy to manage and change around when needed. At the beginning of this project, I *theoretically* knew the advantages of using Arango over the usual PostgreSQL, but after working on it the past few weeks, I can finally appreciate the flexibility of this magical GraphDB system.

You see the query above, *yeah*, that one simple query, **YEAH**, that took me a whole day to figure out. **A WHOOLE DAY**. I know I just said that AQL is much better than normal SQL, because of the flexibility and such, but that doesn't mean it doesn't have its own drawbacks. The biggest drawback of using AQL in my opinion is the learning curve. Since the query language is so versatile, as you can see, it can even return multiple objects and aggregate objects linked by some collection then return them as a list of modified objects, ***Beat that SQL***. By letting the query have such a versatile system, the language used is obviously going to be much more complex. Having *Collecting*, *Aggregating*, and *Grouping* Documents, Graph *Traversals*, Returning custom objects, and other magical operations is like having its own programming language, making learning AQL more like learning a whole new standalone programming language rather a simple querying language.

#### Elastic Search

The most useful progress I've made this last month was creating an Elastic Search wrapper for the back end. I myself feel rather proud of it, even though it took up most of my development time and sucked up most of my enthusiasm for this project due to the problems mentioned at the beginning of this post. Anyways, let's talk about the wrapper.

Sadly the wrapper code is too long to put a snippet in here, and if the code was cut down, it wouldn't make sense at all. So let me just describe how it works. In a broad sense the code for the wrapper is simple, following along the lines of these steps.

- Generate a boiler plate query,
- generate filters, scoring, and functions from the parameters,
- send the query to the ES client, and
- return the result as ES hits.

Yeah as simple as that, exactly as needed, nothing more, nothing less. However, when trying to test my own code, I just realized something. all this time taken to create this system and I forgot a very crucial part. I haven't migrated any data from the DB to the ES. So the testing was halted until I finish creating the migration endpoint for all the data.

Which takes us to the present time. Currently I am working on said migration, and it **SHOULD** be done today, if things go as planned (which they usually don't). That said, I will try my best to finish it up ASAP.

## Refactoring

As this project is finally nearing its final stages, there is one thing that has been in my mind in the past week, Refactoring. the code I made in the course of the last 2 sprints has been horrible. They were all hastily created, used static function, had a jumbled up flow, all around bad (*though it technically still works*). So in this next week, I would actually like to try to refactor my code, with the priorities as follows.

1. Move all handlers and managers to non-static functions,
2. fix flow to a generalized structure (handler&rarr;controller&rarr;util [&rarr;controller&rarr;handler] &rarr;response),
3. use proper mocking for unit testing

Knowing how much work will go into refactoring, I can only hope to finish this, even after pulling several all-nighters.

## In the End

This is likely to be my last post regarding this project, I may later make another one, when the project is actually finalized and can be called 100% done, but don't expect much. On that note, this is farewell. I hope you had as much fun reading these posts as I did writing them. さようなら, ありがとうございました。
