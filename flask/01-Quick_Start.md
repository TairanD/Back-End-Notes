# Quick Start

## Dependency - Werkzeug (wsgi)
```python
from werkzeug.serving import run_simple

def func(environ, start_response):
    print("request")
    pass

if __name__ == '__main__':
    run_simple('127.0.0.1', 5000, func)
```

## Using Flask
```python
from flask import Flask

app = Flask(__name__)

@app.route('/index')
def index():
    return 'hello world'

if __name__ == '__main__':
    app.run()
```

## Summary
- Flask is implemented based on werkzeug's wsgi.
- Once user's request arrive, `app.__call__` method will be executed.
- Standard Procedure of using flask:
  - create Flask object
  - combine routers and views together
  - execute `app.run()`