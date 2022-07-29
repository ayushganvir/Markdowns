# Flask Socketio 
For bi driectional websockets.

## Installation on your env
```sh
pip install flask-socketio
```

## Adding socketio to your flask app 

```py
from flask import Flask, render_template
from flask_socketio import SocketIO

app = Flask(__name__)
app.config['SECRET_KEY'] = 'secret!'
socketio = SocketIO(app)

if __name__ == '__main__':
    socketio.run(app)
```
## For the client side
You can use CDN for socketio library.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket.io.js" integrity="sha512-q/dWJ3kcmjBLU4Qc47E4A9kTB4m3wuTY7vkFJDTZKjTs8jhyGQnaUrxa0Ytd0ssMZhbNua9hE+E7Qv1j+DyZwA==" crossorigin="anonymous"></script>
<script type="text/javascript" charset="utf-8">
    var socket = io();
    socket.on('connect', function() {
        socket.emit('my event', {data: 'I\'m connected!'});
    });
</script>
```

Both client and server can emit events to each other. The events can have some actions triggered by them.

## Connection events

```py
@socketio.on('connect')
def test_connect(auth):
    emit('my response', {'data': 'Connected'})

@socketio.on('disconnect')
def test_disconnect():
    print('Client disconnected')
```
Events are send though emit() function and have the event name and optionally data that would be send with the event to the other end.

The client side of the code responding to this event will look like.

```js
 socket.on('disconnect', function() {
            console.log("Disconnected");
        });
        
        socket.on('connect', function() {
            console.log('Socket', socket.id)
            console.log("Connection made>>>>>>");
            socket.emit("register");
            $("#disconnect").show();
            $("#connect").hide();
        });
```

# Broadcasting

Events that are needed to be broadcasted can be emitted:

```py
    emit('my response', data, broadcast=True)
```

##  Background Tasks

```py
import uuid
from flask import *
from flask_socketio import *
import datetime, time
from threading import Lock

app = Flask(__name__)
app.config['SECRET_KEY'] = 'some super secret key!'
async_mode = None
socketio = SocketIO(app, logger=True, ping_interval=(2,1), async_mode=async_mode)
thread = None
thread_lock = Lock()


def client_ping(t = 60):
    while True:
        socketio.sleep(t)
        date_time = datetime.datetime.now().strftime("%H:%M:%S")
        socketio.emit('minutely_message',
                      {'message': 'Connected on time' + date_time + ' See you in a Minute'})
    

@socketio.on('connect')
def connect():
    socketio.start_background_task(client_ping)
    print("Connect")

```


Here socketio.sleep() is non blocking in nature.

