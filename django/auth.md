# Authentication

Handles user accounts, groups, permissions and cookie-based user sessions. Handles both authentication and authorization.

The auth system consists of:

1. Users
2. Permissions: Binary (yes/no) flags designating whether a user may perform a certain task.
3. Groups: A generic way of applying labels and permissions to more than one user.
4. A configurable password hashing system
5. Forms and view tools for logging in users, or restricting content
6. A pluggable backend system

## Installation

Consist of two items listed in your INSTALLED_APPS setting:

1. 'django.contrib.auth' contains the core of the authentication framework, and its default models.
2. 'django.contrib.contenttypes' is the Django content type system, which allows permissions to be associated with models you create.

Middlewares :
1. SessionMiddleware
2. AuthenticationMiddleware

## User Objects
Has fields :
1. username
2. password
3. email
4. first_name
5. last_name

## Creating User
The most direct way to create users is to use the included create_user() / createsuperuser  helper function.

## Passwords
Django does not store raw (clear text) passwords on the user model, but only a hash.
Django provides a flexible password storage system and uses PBKDF2 by default.

Password is saved as:
```python
<algorithm>$<iterations>$<salt>$<hash>
```
You can use different password hashers defined in settings.py
```python
PASSWORD_HASHERS = [
'django.contrib.auth.hashers.PBKDF2PasswordHasher',

]
```
## Authenticating users
```python
authenticate(request=None, **credentials)¶
```

