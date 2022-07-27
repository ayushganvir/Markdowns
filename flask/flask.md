# Flask

## Werkzeug
- WSGI toolkit

## Jinja2
- Templating engine

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
   return 'Hello World’

if __name__ == '__main__':
   app.run()
```

The route() function of the Flask class is a decorator, which tells the application which URL should call the associated function.
```python
app.route(rule, options)
```

The rule parameter represents URL binding with the function.

The options is a list of parameters to be forwarded to the underlying Rule object.

Finally the run() method of Flask class runs the application on the local development server.
Turn debug = True in developent to reflect code changes.

```python
app.run(host, port, debug=True, options)
```
Another way to route without using decorator ia by using app.add_url_rule().

```python
def hello_world():
   return ‘hello world’
app.add_url_rule(‘/’, ‘hello’, hello_world)
```

To build a URL dynamically,add variable parts to the rule parameter. This variable part is marked as <variable-name>. It is passed as a keyword argument to the function with which the rule is associated.
```python
@app.route('/hello/<name>')
def hello_name(name):
   return 'Hello %s!' % name

@app.route('/blog/<int:postID>')
def show_blog(postID):
   return 'Blog Number %d' % postID
```
## redirect() and url_for()
url_for() function is very useful for dynamically building a URL for a specific function. The function accepts the name of a function as first argument, and one or more keyword arguments, each corresponding to the variable part of URL.

```python
@app.route('/admin')
def hello_admin():
   return 'Hello Admin'

@app.route('/guest/<guest>')
def hello_guest(guest):
   return 'Hello %s as Guest' % guest

@app.route('/user/<name>')
def hello_user(name):
   if name =='admin':
      return redirect(url_for('hello_admin'))
   else:
      return redirect(url_for('hello_guest',guest = name))
```
# HTTP Methods

```html
<html>
   <body>
      <form action = "http://localhost:5000/login" method = "post">
         <p>Enter Name:</p>
         <p><input type = "text" name = "nm" /></p>
         <p><input type = "submit" value = "submit" /></p>
      </form>
   </body>
</html>
```

```python

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST':
      user = request.form['nm']
      return redirect(url_for('success',name = user))
   else:
      user = request.args.get('nm')
      return redirect(url_for('success',name = user))
```

## Jinja2 template
[refer](https://www.tutorialspoint.com/flask/flask_templates.htm)

```python
@app.route('/hello/<int:score>')
def hello_name(score):
   return render_template('hello.html', marks = score)
```

```html
<!doctype html>
<html>
   <body>
      {% if marks>50 %}
         <h1> Your result is pass!</h1>
      {% else %}
         <h1>Your result is fail</h1>
      {% endif %}
   </body>
</html>
```
## Static Files
During the development,  files are served from static folder in your package or next to your module and it will be available at /static on the application.

```python
@app.route("/")
def index():
   return render_template("index.html")
```

```html
<html>
   <head>
      <script type = "text/javascript" 
         src = "{{ url_for('static', filename = 'hello.js') }}" ></script>
   </head>
   
   <body>
      <input type = "button" onclick = "sayHello()" value = "Say Hello" />
   </body>
</html>
```

hello.js contains sayHello() function.
```js
function sayHello() {
   alert("Hello World")
}
```

## Request object
- Form - dictionary object containing key and value pairs of form parameters and their values.
- args - parsed contents of query string which is part of URL after question mark (?).
- Cookies
- files
- Method

## Cookies
A cookie is stored on a client’s computer in the form of a text file. Its purpose is to remember and track data pertaining to a client’s usage for better visitor experience and site statistics.

A Request object contains a cookie’s attribute. It is a dictionary object of all the cookie variables and their corresponding values, a client has transmitted. In addition to it, a cookie also stores its expiry time, path and domain name of the site.

In Flask, cookies are set on response object. Use make_response() function to get response object from return value of a view function. After that, use the set_cookie() function of response object to store a cookie.

Reading back a cookie is easy. The get() method of request.cookies attribute is used to read a cookie.

```python
@app.route('/')
def index():
   return render_template('index.html')
```
This HTML page contains one text input.
```html
<html>
   <body>
      <form action = "/setcookie" method = "POST">
         <p><h3>Enter userID</h3></p>
         <p><input type = 'text' name = 'nm'/></p>
         <p><input type = 'submit' value = 'Login'/></p>
      </form>
   </body>
</html>
```
The Form is posted to ‘/setcookie’ URL. The associated view function sets a Cookie name userID and renders another page.

```python
@app.route('/setcookie', methods = ['POST', 'GET'])
def setcookie():
   if request.method == 'POST':
   user = request.form['nm']
   
   resp = make_response(render_template('readcookie.html'))
   resp.set_cookie('userID', user)
   
   return resp
```
‘readcookie.html’ contains a hyperlink to another view function getcookie(), which reads back and displays the cookie value in browser.

```python
@app.route('/getcookie')
def getcookie():
   name = request.cookies.get('userID')
   return '<h1>welcome '+name+'</h1>'
```
