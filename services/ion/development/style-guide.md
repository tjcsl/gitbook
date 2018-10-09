# Style Guide

The Ion code base generally follows the guidelines as set forth by [PEP8](https://www.python.org/dev/peps/pep-0008/).

## Main Points

* Indent using 4 spaces.
* Use underscores in favor of camel case for all names except the names of classes.
* Limit all lines to a maximum of 79 characters.
* Limit the line length of docstrings or comments to 72 characters.
* Separate top-level functions and class definitions with two blank lines.
* Separate method definitions inside a class with a single blank line.
* Use two spaces before inline comment and one space between the pound sign and comment/
* Use a plugin for your text editor to check for/remind you of PEP8 conventions.
* Capitalize and punctuate comments and git commit messages properly.

## Imports

* Group imports in the following order:
  1. Standard library imports
  2. Imports from core Django
  3. Related third-party imports
  4. Local application or library specific imports, imports from Django apps
* Avoid using `import *`
* Explicitly import each module used

#### Examples

Standard library imports:

```text
from math import sqrt
from os.path import abspath
```

Core Django imports:

```text
from django.db import models
```

Third-party app imports

```text
from django_extensions.db.models import TimeStampedModel
```

Imports from your apps

```text
from intranet.models import User
```

Explicit relative importsUsed to avoid hardcoding a module's package. This greatly improves portability. Use these when importing from another module in the current app.Absolute importsUsed when importing outside the current app.Implicit relative importsDon't use these. Using them makes it very difficult to change the name of the app, reducing portability.

Good:

```text
from .models import SomeModel  # explicit relative import
from  otherdjangoapp.models import OtherModel  # absolute import
```

Bad:

```text
from currentapp.models import MyModel  # implicit relative import
```

### References

* [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html).
* [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.xml).
* [Google Javascript Style Guide](https://google.github.io/styleguide/javascriptguide.xml).
* [PEP8 Official Python Style Guide](https://www.python.org/dev/peps/pep-0008/).

