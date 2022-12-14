# Easy Environment Variables

An easier way to use environment variables with the correct typecasting in Python.

[![pipeline status](https://gitlab.geekeno.com/utils/easy-env/badges/main/pipeline.svg)](https://gitlab.geekeno.com/utils/easy-env/-/commits/main)
[![coverage report](https://gitlab.geekeno.com/utils/easy-env/badges/main/coverage.svg)](https://gitlab.geekeno.com/utils/easy-env/-/commits/main)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/easy-env-var)](https://pypi.org/project/easy-env-var/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](https://gitlab.geekeno.com/utils/easy-env/-/blob/main/LICENSE)


Since environment variables can only be strings, it usually means we need loads of 
related variables or weird casting in our code. Without [easy_env_var][easy_env_var] that would
look something like this

```.env
CACHE_KEY=default
CACHE_BACKEND=redis_cache.RedisCache
CACHE_LOCATION=redis://localhost:6379/3
DEBUG=true
TIMEOUT=1000
```
```pythonstub
import os

CACHES = {
    os.environ.get("CACHE_KEY", "default"): {
        "BACKEND": os.environ["CACHE_BACKEND"],
        "LOCATION": os.environ["CACHE_LOCATION"],
    }
}
DEBUG = os.environ.get("DEBUG", "false") == "false"
TIMEOUT = int(os.environ.get("TIMEOUT", "300"))
```

With [easy_env_var][easy_env_var] it is as simple as
```.env
CACHES = {"default": {"BACKEND": redis_cache.RedisCache, "LOCATION": redis://localhost:6379/3}}
DEBUG=true
TIMEOUT=1000
```
```pythonstub
from easy_env_var import env

CACHES = env("CACHE", expected_type=dict)
DEBUG = env("DEBUG", expected_type=bool, default=False)
TIMEOUT = env("TIMEOUT", expected_type=int, default=300)
```

### Installation
Simply install using pip by running
```
pip install easy_env_var
```
For Python 3.7 you will also need [typed-ast](https://pypi.org/project/typed-ast/) which
should automatically be installed. For Python 3.8 and above it is not needed.

### Usage
```pythonstub
from easy_env_var import env
FOO = env("foo")  # get the environment variable named foo
BAR = env("bar", expected_type=float)  # cast to the correct data type
try:
    BAZ = env("baz")
except KeyError:
    BAZ = "Not Set"
# or simply pass a default
QUX = env("qux", default="The default value")
```

### Supported data types
Environment variables can be parsed to the following data types: 
* str (this is the default type)
* int
* float
* bool (works with case-insensitive values of True & False)
* Decimal
* list
* dict (the keys need to always be strings)                               
Sets and tuples are not supported since using lists is good enough for most use cases.

### Why easy_env_var?
We wanted to call it [easy_env](https://pypi.org/project/easy-env/) but that's an
existing package.

[easy_env_var]: https://gitlab.geekeno.com/utils/easy-env/
