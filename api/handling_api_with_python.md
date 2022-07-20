# API

```python
import requests

response = requests.get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=USD&order=market_cap_desc&per_page=100&page=1&sparkline=false')
print(response.status_code)
>>> 200
print(response.json())
>>> json response

import json

def jprint(obj):
    # create a formatted string of the Python JSON object
    text = json.dumps(obj, sort_keys=True, indent=4)
    print(text)

jprint(response.json())
```

The json library has two main functions:

1. json.dumps() — Takes in a Python object, and converts (dumps) it to a string.
2. json.loads() — Takes a JSON string, and converts (loads) it to a Python object.


```python
parameters = {
    "lat": 40.71,
    "lon": -74
}
response = requests.get("https://api.open-notify.org/iss-pass.json", params=parameters)

print(response.json())
```