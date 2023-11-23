# QuerySets are Lazy
#python  #django #orm

In this article we explore the lazy QuerySets in Django. Django on Fly.io is pretty sweet! Check it out: [you can be up and running on Fly.io in just minutes.](https://fly.io/docs/django/)

Did you know that QuerySets are lazy?

QuerySets have a hidden gem 💎 called lazy evaluation. It is a powerful feature that allows you to postpone the execution of a database’s query until it’s absolutely necessary.

Let’s dive right in.

## [What are Lazy QuerySets?💭](https://fly.io/django-beats/querysets-are-lazy/#what-are-lazy-querysets) 

When you perform operations on a QuerySet, such as applying a filter, ordering or slicing, Django constructs a query but doesn’t execute it immediately. Instead, the actual database query is executed only when you access the data or explicitly evaluate the QuerySet.

This deferred execution allows for efficient querying and reduces unnecessary database operations.
## [How do Lazy QuerySets work? �](https://fly.io/django-beats/querysets-are-lazy/#how-do-lazy-querysets-work)

Let’s say we have a `Title` model with thousands of records and a `Genre` model with couple dozens:

This is a simplified version of [this](https://github.com/katiayn/django-imdb) project.

```python
class Genre(models.Model):
    name = models.CharField(max_length=255, unique=True)

    def __str__(self):
        return self.name


class Title(models.Model):
    title = models.CharField(max_length=255)
    genre = models.ManyToManyField(Genre, related_name="titles", blank=True)
    year_start = models.PositiveSmallIntegerField(null=True, blank=True)
    imdb_rating = models.FloatField()

    class Meta:
        ordering = ("-imdb_rating",)

    def __str__(self):
        return self.title
```

This is a simplified version of [this](https://github.com/katiayn/django-imdb) project.

By taking advantage of the lazy evaluation, we can efficiently narrow down our results without fetching unnecessary data:

```python
titles = Title.objects.all()  # Query is not executed yet

# Chaining additional operations: filtering, ordering and slicing
titles = titles.filter(imdb_rating__gte=8.5).order_by("-year_start")[:10]

# Access the data
for title in titles:
    print(f"{title.year_start}: {title.title} [{title.imdb_rating}]")
```

Note that we create the QuerySet fetching all titles.

```python
titles = Title.objects.all()
```

This operation returns an unevaluated QuerySet.

Subsequently, we filter, order and slice our QuerySet - fetch titles with `imdb_rating` greater than or equal `8.5`, sort in descending order by `year_start` and return the first `10` titles:

```python
titles = titles.filter(imdb_rating__gte=8.5).order_by("-year_start")[:10]
```

By this point, no operations to the database were actually executed.

Then, we finally access the data by iterating over the results:

```python
for title in titles:
    print(f"{title.year_start}: {title.title} [{title.imdb.rating}]")
```

At this step, the query is finally executed in the database. Note that we ensure that only the filtered, ordered and sliced results are retrieved from the database.

## [Evaluating the Queries](https://fly.io/django-beats/querysets-are-lazy/#evaluating-the-queries)

We can verify locally that the queries aren’t executed yet by [checking the SQL statements](https://docs.djangoproject.com/en/stable/faq/models/#how-can-i-see-the-raw-sql-queries-django-is-running) that Django performed using `connection.queries`:

```bash
python3 manage.py shell
```

```python
>>> from catalogue.models import Title
>>> from django.db import connection
>>>
>>> titles = Title.objects.all()
>>> len(connection.queries)
0
>>> titles = titles.filter(imdb_rating__gte=8.5).order_by("-year_start")[:10]
>>> len(connection.queries)
0
>>> for title in titles:
...     print(f"{title.year_start}: {title.title} [{title.imdb_rating}]")
... 
2022: House of the Dragon [8.5]
2019: Parasite [8.5]
2019: Chernobyl [9.4]
2018: Beat Saber [8.9]
2018: Succession [8.9]
2017: Dark [8.7]
2016: Stranger Things [8.7]
2015: Narcos [8.8]
2014: True Detective [8.9]
2013: Vikings [8.5]
>>> len(connection.queries)
1
>>> connection.queries
[
    {
        'sql': '
            SELECT "catalogue_title"."id", "catalogue_title"."title", "catalogue_title"."year_start", "catalogue_title"."imdb_rating" 
            FROM "catalogue_title" 
            WHERE "catalogue_title"."imdb_rating" >= 8.5 
            ORDER BY "catalogue_title"."year_start" 
            DESC LIMIT 10
        ', 
        'time': '0.002'
    }
]
```

We can confirm that only one query was performed and display it by accessing `connection.queries`.

`connection.queries` is only available if `DEBUG` is `True`.

## [Exploring the Laziness 🔎](https://fly.io/django-beats/querysets-are-lazy/#exploring-the-laziness)

Iterating over a QuerySet is not the only way to trigger an immediate query to the database, some other methods include:

- Accessing specific elements: `titles[0]` or `titles.get(pk=1)`
- Slicing using the `step` parameter: `titles[::2]`
- Counting: `titles.count()`
- Existence check: `titles.exists()` or `if titles:`
- Deleting: `titles.delete()`
- Evaluation methods: `titles.first()`, `titles.last()`, `titles.earliest()`, `titles.latest()`, etc.
- Aggregation over a QuerySet: `titles.aggregate(Min("year_start"))`  
    
- Iterating using `iterator()`: `titles.iterator(chunk_size=3000)`

Those methods also include the asynchronous versions: `aget()`, `acount()`, `aexists()`, `adelete()`, `afirst()`, `alast()`, `alatest()`, `aearliest()`, `aaggregate()`, `aiterator()`, etc.

- Converting a QuerySet to a list: `list(titles)`

On the other hand, besides filtering, ordering or slicing, accessing related objects using the double underscore (`__`) notation, does not immediately fetch the related objects. Instead, they are also lazily fetched when accessed:

```python
>>> from catalogue.models import Title
>>> from django.db import connection
>>> 
>>> titles = Title.objects.all()
>>> titles = titles.filter(imdb_rating__gte=8.5).order_by("-year_start")
>>> titles = titles.filter(genre__name="Action")  # filter related object
>>> len(connection.queries)
0
>>> for title in titles:
...     print(title.title)
... 
House of the Dragon
The Last of Us
Vikings
Game of Thrones
Inception
The Matrix
>>> len(connection.queries)
1
>>> connection.queries
[
    {
        'sql': '
            SELECT "catalogue_title"."id", "catalogue_title"."title", "catalogue_title"."year_start", "catalogue_title"."imdb_rating" 
            FROM "catalogue_title" 
            INNER JOIN "catalogue_title_genre" 
                ON ("catalogue_title"."id" = "catalogue_title_genre"."title_id") 
            INNER JOIN "catalogue_genre" 
                ON ("catalogue_title_genre"."genre_id" = "catalogue_genre"."id") 
            WHERE ("catalogue_title"."imdb_rating" >= 8.5 AND "catalogue_genre"."name" = \'Action\') 
            ORDER BY "catalogue_title"."year_start" DESC
        ', 
        'time': '0.003'
    }
]
```

The `Title` model is filtered by `genre`, which is a `ManyToManyField`. Since those fields are not explicitly accessed, no additional queries are performed.

Keep in mind that once a query is executed, the retrieved data is stored in memory, and subsequent operation on that QuerySet use the “cached” data rather than querying the database again. This internal caching in memory mechanism is limited to the scope of the QuerySet instance and does not persist beyond the lifetime of that specific QuerySet.

One exception worth mentioning is the [`iterator()`](https://docs.djangoproject.com/en/stable/ref/models/querysets/#iterator). In order to avoid loading all the objects into memory at once, `iterator()` fetches a chunk of data directly from the database at a time, without doing any caching at the QuerySet level - take into account that this approach can triggers multiple queries to the database.

This method is particularly ideal for large amount of objects that we only need to access once, resulting in higher performance and reducing the memory usage.

Always evaluate the trade-offs between memory efficiency and the potential for additional database queries.

However, modifying the QuerySet will trigger a new query to the database.

When it comes to QuerySets in Django, understanding which methods trigger an immediate execution and which ones do not, helps us minimizing unnecessary queries and optimize performance.

## [Wrap-Up 📄](https://fly.io/django-beats/querysets-are-lazy/#wrap-up)

Lazy QuerySets are useful when we need to construct dynamic queries based on runtime conditions or user inputs. It’s also ideal for implementing pagination by retrieving and displaying only a subset of records at a time. Those are just a few examples on how [lazy QuerySets](https://docs.djangoproject.com/en/stable/topics/db/queries/#querysets-are-lazy) might come in handy.

While it offers performance optimizations and flexibility, excessive chaining or complex query construction can make our code harder to read and maintain. It’s always important to keep a balance when applying lazy evaluation to our QuerySets.