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

