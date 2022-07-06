# Django Rest framework
Wrapper over django to create **API**

## Three Stages before creating an API:

1. Converting models data to XML/JSON (Serialization)
2. Rendering this data to the view.
3. Creating URL for mapping to the viewset.

Assuming you have install djangorestframework 
```
pip install djangorestframework
```
in your environment.

## Steps involved:
1. Add rest_framework to INSTALLED_APPS
2. Create an App and Model
3. Serialization
    - Serializers allow complex data such as querysets and model instances to be converted to native Python datatypes that can then be easily rendered into JSON, XML or other content types.
    - Serializers also provide deserialization, allowing parsed data to be converted back into complex types, after first validating the incoming data. Let’s start creating a serializer, in file *apis/serializers.py* .
4. Create a viewset
    - Similar to Django views
5. Define URLs of API
6. Run server and check API

