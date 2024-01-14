# IPython extension to mask sensitive data

Simple ipython extension to mask the username or other sensitive data
from notebook cell outputs.  It may be useful when printing
or displaying sensitive information in public notebooks.
The extension modifies the ipython display system to mask textual outputs
from printing, logging and the native display system.

## Usage

To load the extension use the magic command `%load_ext nbmask`. The extension will automatically
mask the notebook display textual outputs without any further magic commands. By default the
extension will mask the username.

```python
import os
import logging

from pathlib import Path

from IPython.display import display

logging.basicConfig(level="DEBUG", force=True)

%load_ext nbmask

username = os.getenv('USER')

print("My name is {username}!")
# >>> My name is ...!

documents = Path(f"/Users/{username}/Documents")
print(documents)
# >>> PosixPath('/Users/.../Documents')
```


You can add more secrets with the `%nbmask` magic line command
using the ipython automatic `$name` variable expansion.


```python
TOKEN = my_secret_token()

%nbmask "$TOKEN"

credentials = dict(user=username, token=TOKEN)

print(credentials)
# >>> {'user': '...', 'token': '...'}

logging.debug("Token is %s", TOKEN)
# >>> DEBUG:root:Token is ...

```

To mask `print` or `pprint` outputs the extension include a `%%masked` cell magic
but is is no longer and it will be removed.


## Example

See nbmask-tests.ipynb in `extras`

## Installation

You can install the current version of this package with pip

```console
pip install nbmask
```

## Changelog

### 0.0.4
- Cell magic `%%masked` is no longer needed. Will be removed

### 0.0.3
- Masking pattern is cached
