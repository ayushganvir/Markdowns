# Queries

```python
from blog_app import Post
posts = Post.object.all()
```

Here post is a Queryset of length equal to the number of Post objects
iterate over each for each object

## filter()

Returns a new QuerySet containing objects that match the given lookup parameters.

Multiple parameters are joined via AND in the underlying SQL statement.

If you need to execute more complex queries (for example, queries with OR statements), you can use Q objects (*args).

```python
post = Post.objects.filter(title__startswith = 'ayush')  
[<Queryset>]
```

## exclude()
Returns a new QuerySet containing objects that do not match the given lookup parameters.

## order_by()

 results returned by a QuerySet are ordered by the ordering tuple given by the ordering option in the model’s Meta. You can override this on a per-QuerySet basis by using the order_by method.

You can also order by query expressions by calling asc() or desc() on the expression
 ```python
 Entry.objects.filter(pub_date__year=2005).order_by('-pub_date', 'headline').desc()
 ```

 ## distinct()

 Returns a new QuerySet that uses SELECT DISTINCT in its SQL query. This eliminates duplicate rows from the query results.

 ## values()
Returns a QuerySet that returns dictionaries, rather than model instances, when used as an iterable.

Each of those dictionaries represents an object, with the keys corresponding to the attribute names of model objects.
 ```python 
 Blog.objects.filter(name__startswith='Beatles').values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
```
## values_list()
This is similar to values() except that instead of returning dictionaries, it returns tuples when iterated over. 

A common need is to get a specific field value of a certain model instance. To achieve that, use values_list() followed by a get() call.

## dates(), datetimes()
Returns a QuerySet that evaluates to a list of datetime.date, datetime.datetime objects representing all available dates of a particular kind within the contents of the QuerySet.

```python
Entry.objects.dates('pub_date', 'year')
[datetime.date(2005, 1, 1)]
```
## all()
Returns a copy of the current QuerySet (or QuerySet subclass). This can be useful in situations where you might want to pass in either a model manager or a QuerySet and do further filtering on the result.

## union(), intersection(), difference()
Uses SQL’s UNION operator to combine the results of two or more QuerySets. The UNION operator selects only distinct values by default. To allow duplicate values, use the all=True argument.

```python
qs1.union(qs2, qs3)
```

union(), intersection(), and difference() return model instances of the type of the first QuerySet even if the arguments are QuerySets of other models. Passing different models works as long as the SELECT list is the same in all QuerySets (at least the types, the names don’t matter as long as the types are in the same order). In such cases, you must use the column names from the first QuerySet in QuerySet methods applied to the resulting QuerySet.

```python
qs1 = Author.objects.values_list('name')
qs2 = Entry.objects.values_list('headline')
qs1.union(qs2).order_by('name')
```
## select_related()
Returns a QuerySet that will “follow” foreign-key relationships, selecting additional related-object data when it executes its query. This is a performance booster which results in a single more complex query but means later use of foreign-key relationships won’t require database queries.

```python
# Hits the database.
e = Entry.objects.get(id=5)

# Hits the database again to get the related Blog object.
b = e.blog
# Hits the database.
e = Entry.objects.select_related('blog').get(id=5)

# Doesn't hit the database, because e.blog has been prepopulated
# in the previous query.
b = e.blog
```
## prefetch_related()
Returns a QuerySet that will automatically retrieve, in a single batch, related objects for each of the specified lookups.

This has a similar purpose to select_related, in that both are designed to stop the deluge of database queries that is caused by accessing related objects, but the strategy is quite different.

select_related works by creating an SQL join and including the fields of the related object in the SELECT statement. For this reason, select_related gets the related objects in the same database query. However, to avoid the much larger result set that would result from joining across a ‘many’ relationship, select_related is limited to single-valued relationships - foreign key and one-to-one.

prefetch_related, on the other hand, does a separate lookup for each relationship, and does the ‘joining’ in Python. This allows it to prefetch many-to-many and many-to-one objects, which cannot be done using select_related, in addition to the foreign key and one-to-one relationships that are supported by select_related.
## extra()
Sometimes, the Django query syntax by itself can’t easily express a complex WHERE clause. For these edge cases, Django provides the extra() QuerySet modifier — a hook for injecting specific clauses into the SQL generated by a QuerySet.

```python
qs.extra(select=None, where=None, params=None, tables=None, order_by=None, select_params=None)
```

## defer(), only()
If you are using the results of a queryset in some situation where you don’t know if you need those particular fields when you initially fetch the data, you can tell Django not to retrieve them from the database.
```python 
Entry.objects.defer("headline", "body")
```
The only() method is more or less the opposite of defer(). 
```python
Person.objects.only("name")
```
Since defer() acts incrementally (adding fields to the deferred list), you can combine calls to only() and defer() and things will behave logically:

```python
# Final result is that everything except "headline" is deferred.
Entry.objects.only("headline", "body").defer("body")

# Final result loads headline and body immediately (only() replaces any
# existing set of fields).
Entry.objects.defer("body").only("headline", "body")
```
## using()

This method is for controlling which database the QuerySet will be evaluated against if you are using more than one database. The only argument this method takes is the alias of a database, as defined in DATABASES.

```python
# queries the database with the 'default' alias.
>>> Entry.objects.all()

# queries the database with the 'backup' alias
>>> Entry.objects.using('backup')
```
## raw()

Takes a raw SQL query, executes it, and returns a django.db.models.query.RawQuerySet instance. This RawQuerySet instance can be iterated over just like a normal QuerySet to provide object instances.
## Operators that return new QuerySets

## AND (&)
Combines two QuerySets using the SQL AND operator.

The following are equivalent:
```python
Model.objects.filter(x=1) & Model.objects.filter(y=2)
Model.objects.filter(x=1, y=2)
from django.db.models import Q
Model.objects.filter(Q(x=1) & Q(y=2))
```
## OR (|)
Combines two QuerySets using the SQL OR operator.

The following are equivalent:

```python
Model.objects.filter(x=1) | Model.objects.filter(y=2)
from django.db.models import Q
Model.objects.filter(Q(x=1) | Q(y=2))
```
## get()
If you know there is only one object that matches your query, you can use the get() method on a Manager which returns the object directly:
```python
>>> one_entry = Entry.objects.get(pk=1)
```
Note that there is a difference between using get(), and using filter() with a slice of [0]. If there are no results that match the query, get() will raise a DoesNotExist exception. This exception is an attribute of the model class that the query is being performed on - so in the code above, if there is no Entry object with a primary key of 1, Django will raise Entry.DoesNotExist.

## create()
A convenience method for creating an object and saving it all in one step. Thus:
```python
p = Person.objects.create(first_name="Bruce", last_name="Springsteen")
```
## get_or_create()
Returns a tuple of (object, created), where object is the retrieved or created object and created is a boolean specifying whether a new object was created.
```python
book.chapters.get_or_create(title="Telemachus")
(<Chapter: Telemachus>, True)
>>> book.chapters.get_or_create(title="Telemachus")
(<Chapter: Telemachus>, False)
```

## update_or_create()
A convenience method for updating an object with the given kwargs, creating a new one if necessary. The defaults is a dictionary of (field, value) pairs used to update the object. The values in defaults can be callables.

Returns a tuple of (object, created), where object is the created or updated object and created is a boolean specifying whether a new object was created.
## count()
Returns an integer representing the number of objects in the database matching the QuerySet.

Example:
```python
# Returns the total number of entries in the database.
Entry.objects.count()

# Returns the number of entries whose headline contains 'Lennon'
Entry.objects.filter(headline__contains='Lennon').count()
```
## latest(), earliest()
Returns the latest object in the table based on the given field(s).

This example returns the latest Entry in the table, according to the pub_date field:
```python
Entry.objects.latest('pub_date')
```
## first(), last()
Returns the first object matched by the queryset, or None if there is no matching object. If the QuerySet has no ordering defined, then the queryset is automatically ordered by the primary key. This can affect aggregation results as described in Interaction with order_by().

Example:
```python
p = Article.objects.order_by('title', 'pub_date').first()
```
## exists(), contains()
Returns True if the QuerySet contains any results, and False if not. This tries to perform the query in the simplest and fastest way possible, but it does execute nearly the same query as a normal QuerySet query.

exists() is useful for searches relating to the existence of any objects in a QuerySet, particularly in the context of a large QuerySet.

To find whether a queryset contains any items:
```python
if some_queryset.exists():
    print("There is at least one object in some_queryset")
```
Which will be faster than:
```python
if some_queryset:
    print("There is at least one object in some_queryset")
```
## update
Performs an SQL update query for the specified fields, and returns the number of rows matched (which may not be equal to the number of rows updated if some rows already have the new value).
The update() method is applied instantly, and the only restriction on the QuerySet that is updated is that it can only update columns in the model’s main table, not on related models. You can’t do this, for example:
```python
>>> Entry.objects.update(blog__name='foo') # Won't work!
```
Filtering based on related fields is still possible, though:
```python
>>> Entry.objects.filter(blog__id=1).update(comments_on=True)
```
## delete()
Performs an SQL delete query on all rows in the QuerySet and returns the number of objects deleted and a dictionary with the number of deletions per object type.

The delete() is applied instantly. You cannot call delete() on a QuerySet that has had a slice taken or can otherwise no longer be filtered.

## Field Lookups
Field lookups are how you specify the meat of an SQL WHERE clause. They’re specified as keyword arguments to the QuerySet methods filter(), exclude() and get().
For all lookups:
https://docs.djangoproject.com/en/4.0/ref/models/querysets/#field-lookups

1. exact
2. iexact
3. contains
4. icontains
5. in
6. gt
7. gte
8. lt
9. lte
10. startswith
11. istartswith
12. endswith
13. iendswith
14. range

```python
import datetime
start_date = datetime.date(2005, 1, 1)
end_date = datetime.date(2005, 3, 31)
Entry.objects.filter(pub_date__range=(start_date, end_date))
```
15. date, year, month, day
16. regex, isnull, hour, minute, second

## Aggregate in models
```python
# Total number of books.
>>> Book.objects.count()
2452

# Total number of books with publisher=BaloneyPress
>>> Book.objects.filter(publisher__name='BaloneyPress').count()
73

# Average price across all books.
>>> from django.db.models import Avg
>>> Book.objects.all().aggregate(Avg('price'))
{'price__avg': 34.35}

# Max price across all books.
>>> from django.db.models import Max
>>> Book.objects.all().aggregate(Max('price'))
{'price__max': Decimal('81.20')}

# Difference between the highest priced book and the average price of all books.
>>> from django.db.models import FloatField
>>> Book.objects.aggregate(
...     price_diff=Max('price', output_field=FloatField()) - Avg('price'))
{'price_diff': 46.85}

# All the following queries involve traversing the Book<->Publisher
# foreign key relationship backwards.

# Each publisher, each with a count of books as a "num_books" attribute.
>>> from django.db.models import Count
>>> pubs = Publisher.objects.annotate(num_books=Count('book'))
>>> pubs
<QuerySet [<Publisher: BaloneyPress>, <Publisher: SalamiPress>, ...]>
>>> pubs[0].num_books
73

# Each publisher, with a separate count of books with a rating above and below 5
>>> from django.db.models import Q
>>> above_5 = Count('book', filter=Q(book__rating__gt=5))
>>> below_5 = Count('book', filter=Q(book__rating__lte=5))
>>> pubs = Publisher.objects.annotate(below_5=below_5).annotate(above_5=above_5)
>>> pubs[0].above_5
23
>>> pubs[0].below_5
12

# The top 5 publishers, in order by number of books.
>>> pubs = Publisher.objects.annotate(num_books=Count('book')).order_by('-num_books')[:5]
>>> pubs[0].num_books
1323

```
## Prefetch
class Prefetch(lookup, queryset=None, to_attr=None)

The Prefetch() object can be used to control the operation of prefetch_related().

The lookup argument describes the relations to follow and works the same as the string based lookups passed to prefetch_related(). For example:
```python
>>> from django.db.models import Prefetch
>>> Question.objects.prefetch_related(Prefetch('choice_set')).get().choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
# This will only execute two queries regardless of the number of Question
# and Choice objects.
>>> Question.objects.prefetch_related(Prefetch('choice_set')).all()
<QuerySet [<Question: What's up?>]>
```
## F Expression
Instances of F() act as a reference to a model field within a query. These references can then be used in query filters to compare the values of two different fields on the same model instance.

For example, to find a list of all blog entries that have had more comments than pingbacks, we construct an F() object to reference the pingback count, and use that F() object in the query:
```python
>>> from django.db.models import F
>>> Entry.objects.filter(number_of_comments__gt=F('number_of_pingbacks'))
```
## Q Objects
Keyword argument queries – in filter(), etc. – are “AND”ed together. If you need to execute more complex queries (for example, queries with OR statements), you can use Q objects.

A Q object (django.db.models.Q) is an object used to encapsulate a collection of keyword arguments. These keyword arguments are specified as in “Field lookups” above.

For example, this Q object encapsulates a single LIKE query:
```python
from django.db.models import Q
Q(question__startswith='What')
```
Q objects can be combined using the & and | operators. When an operator is used on two Q objects, it yields a new Q object.

For example, this statement yields a single Q object that represents the “OR” of two "question__startswith" queries:
```python
Q(question__startswith='Who') | Q(question__startswith='What')
```
## aggregate()
 What we need is a way to calculate summary values over the objects that belong to this QuerySet. This is done by appending an aggregate() clause onto the QuerySet:
```python
>>> from django.db.models import Avg
>>> Book.objects.aggregate(Avg('price'))
{'price__avg': 34.35}
```
## Database Transaction

## ATOMIC_REQUESTS 
A common way to handle transactions on the web is to wrap each request in a transaction. Set ATOMIC_REQUESTS to True in the configuration of each database for which you want to enable this behavior.

When ATOMIC_REQUESTS is enabled, it’s still possible to prevent views from running in a transaction.

## non_atomic_requests()

This decorator will negate the effect of ATOMIC_REQUESTS for a given view:
```python
from django.db import transaction

@transaction.non_atomic_requests
def my_view(request):
    do_stuff()
```
## atomic()
Atomicity is the defining property of database transactions. atomic allows us to create a block of code within which the atomicity on the database is guaranteed. If the block of code is successfully completed, the changes are committed to the database. If there is an exception, the changes are rolled back.
atomic is usable both as a decorator:
```python
from django.db import transaction

@transaction.atomic
def viewfunc(request):
    # This code executes inside a transaction.
    do_stuff()
```
and as a context manager:
```python
from django.db import transaction

def viewfunc(request):
    # This code executes in autocommit mode (Django's default).
    do_stuff()

    with transaction.atomic():
        # This code executes inside a transaction.
        do_more_stuff()
```
## Deactivating transaction management
You can totally disable Django’s transaction management for a given database by setting AUTOCOMMIT to False in its configuration. If you do this, Django won’t enable autocommit, and won’t perform any commits. You’ll get the regular behavior of the underlying database library.

# on_commit()
Django provides the on_commit() function to register callback functions that should be executed after a transaction is successfully committed:

>on_commit(func, using=None)

Pass any function (that takes no arguments) to on_commit():
```python
from django.db import transaction

def do_something():
    pass  # send a mail, invalidate a cache, fire off a Celery task, etc.

transaction.on_commit(do_something)
```
## Savepoints
A savepoint is a marker within a transaction that enables you to roll back part of a transaction, rather than the full transaction. Savepoints are available with the SQLite, PostgreSQL, Oracle, and MySQL (when using the InnoDB storage engine) backends. Other backends provide the savepoint functions, but they’re empty operations – they don’t actually do anything.
https://docs.djangoproject.com/en/4.0/topics/db/transactions/#topics-db-transactions-savepoints
## Defining Databases in Settings
This is done using the DATABASES setting. This setting maps database aliases, which are a way to refer to a specific database throughout Django, to a dictionary of settings for that specific connection. The settings in the inner dictionaries are described fully in the DATABASES documentation.
https://docs.djangoproject.com/en/4.0/topics/db/multi-db/
