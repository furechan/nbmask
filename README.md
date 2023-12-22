# IPython extension to mask sensitive data

Simple ipython extension to mask the username or other sensitive data
from the ipython notebook outputs.  It may be useful when printing
or displaying sensitive information in public notebooks.

## Usage

To load the extension use the magic command `%load_ext nbmask`. The extension will automatically
mask the notebook display textual outputs without any further magic commands. By default the
extension will mask the username.

```python
import os

from pathlib import Path

from IPython.display import display

%load_ext nbmask

username = os.getenv('USER')
message = f"My name is {username}!"
documents = Path(f"/Users/{username}/Documents")

display(username) # >>> '...'

display(message) # >>> 'My name is ...!'

display(documents) # >>> PosixPath('/Users/.../Documents')
```


You can add more secrets with the `%nbmask` magic line command
using the ipython automatic `$name` variable expansion.


```python
TOKEN = my_secret_token()

%nbmask "$TOKEN"

credentials = dict(token=TOKEN)

credentials # >>> {'token': '...'}
```

To mask `print` or `pprint` outputs you need to explicitely use the `%%masked` cell magic
for any cell that prints sensitive data.

```python
%%masked

message = f"My name is {username}!"

print(message) # >>> My name is ...!
```


The `%%masked` cell magic can also be used to mask logging outputs.

```python
%%masked

import logging

logging.basicConfig(level="DEBUG", force=True)

logging.debug("Used token %s", TOKEN) # >>> DEBUG:root:Used token ...
```


## Example

See nbmask-tests.ipynb in `extras`


## Installation

You can install the current version of this package with pip

```console
python -mpip install git+https://github.com/furechan/nbmask.git
```

## Changelog

### 0.0.3
- Masking pattern is cached
