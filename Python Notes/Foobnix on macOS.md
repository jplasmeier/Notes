# Foobnix on macOS

a guide by J. Plasmeier 

## Installation

Error: `sh: msgfmt: command not found`

Solution: 

```
brew install gettext
brew link getttext
```

Error: `ImportError: No module named gi`

Solution:

MAYBE: `brew install pygobject3`

```
pip install gi
```

Error: `AttributeError: 'module' object has no attribute 'require_version'`

Solution: 

MAYBE: `brew install gtk+3`
