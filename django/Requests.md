# Handling HTTP Requests
https://docs.djangoproject.com/en/4.0/topics/http/urls/

## render()
>render(request, template_name, context=None, content_type=None, status=None, using=None)

Combines a given template with a given context dictionary and returns an HttpResponse object with that rendered text.

Django does not provide a shortcut function which returns a TemplateResponse because the constructor of TemplateResponse offers the same level of convenience as render().
### Required arguments
### request
The request object used to generate this response.
### template_name
The full name of a template to use or sequence of template names. If a sequence is given, the first template that exists will be used. See the template loading documentation for more information on how templates are found.
### Optional arguments
### context
A dictionary of values to add to the template context. By default, this is an empty dictionary. If a value in the dictionary is callable, the view will call it just before rendering the template.
### content_type
The MIME type to use for the resulting document. Defaults to 'text/html'.
### status
The status code for the response. Defaults to 200.
using
The NAME of a template engine to use for loading the template.
## redirect()

>redirect(to, *args, permanent=False, **kwargs)

Returns an HttpResponseRedirect to the appropriate URL for the arguments passed.

The arguments could be:

A model: the model’s get_absolute_url() function will be called.

A view name, possibly with arguments: reverse() will be used to reverse-resolve the name.

An absolute or relative URL, which will be used as-is for the redirect location.

By default issues a temporary redirect; pass permanent=True to issue a permanent redirect.

## Middleware
Middleware is a framework of hooks into Django’s request/response processing. It’s a light, low-level “plugin” system for globally altering Django’s input or output.

Each middleware component is responsible for doing some specific function. For example, Django includes a middleware component, AuthenticationMiddleware, that associates users with requests using sessions.

## Caching

Django has different caching granularity
- Output of specific views
- pieces that are difficult to produce
- entire site

Caching also works with downstream cache,
> **downstream cache** | These are the types of caches that you don’t directly control but to which you can provide hints (via HTTP headers) about which parts of your site should be cached, and how.

## Redis
https://docs.djangoproject.com/en/4.0/topics/cache/#redis
- Redis an in-memory database that can be used for caching. To begin you’ll need a Redis server running either locally or on a remote machine.

- After setting up the Redis server, you’ll need to install Python bindings for Redis. **redis-py** is the binding supported natively by Django. Installing the additional **hiredis-py** package is also recommended.

To use Redis as your cache backend with Django:

1. Set BACKEND to django.core.cache.backends.redis.RedisCache.

2. Set LOCATION to the URL pointing to your Redis instance, using the appropriate scheme. 

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.redis.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379',
    }
}
```
f you have multiple Redis servers set up in the replication mode, you can specify the servers either as a semicolon or comma delimited string, or as a list. While using multiple servers, write operations are performed on the first server (leader). 

## Database Caching
Django can store its cached data in your database. This works best if you’ve got a fast, well-indexed database server.

To use a database table as your cache backend:

1. Set BACKEND to django.core.cache.backends.db.DatabaseCache
2. Set LOCATION to tablename, the name of the database table. This name can be whatever you want, as long as it’s a valid table name that’s not already being used in your database.
```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'my_cache_table',
    }
}
```
Unlike other cache backends, the database cache does not support automatic culling of expired entries at the database level. Instead, expired cache entries are culled each time add(), set(), or touch() is called.

## Creating cache table

```python
python manage.py createcachetable
```
