# Django Rest Framework
Wrapper over django to create **API**

## REST API
Representational state transfer are set of constraints for making web services.
REST is preferred over SOAP(Simple Object Access Protocol).

## Architectural Constraints of RESTful API:
 There are six architectural constraints which makes any web service are listed below:

- Uniform Interface
- Stateless
- Cacheable
- Client-Server
- Layered System
- Code on Demand

## Working

1. Request sent from client to server in form of URL as HTTP GET, POST, PUT, DELETE
2. Response of server in HTML, JSON, XML, Image.

## Three Stages before creating an API:

1. Converting models data to XML/JSON (Serialization)
2. Rendering this data to the view.
3. Creating URL for mapping to the viewset.

Assuming you have to install djangorestframework 
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

