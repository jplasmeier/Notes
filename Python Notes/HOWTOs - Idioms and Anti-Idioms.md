# Python Notes

## HOWTO: Idioms and Anti-Idiom notes

Things you should and should not do with the Python Language.

### Language Constructs you should _not_ use

* `from module import *`

Invalid inside function definition, and even if it is accepted it's slow and stupid. Valid at the module level, but still slow and stupid. Also, you can accidentally shadow a builtin and break things. Only take what you need from that module. If you're in an interactive shell it's probably ok.

* `exec` or `execfile()` without an explicit dictionary

Will evaulate code in the current environment. Similarly as above, this can overwrite variable names in your code.

* `from module import name1, name2`

This causes these names to exist in two namespaces at once. I'm guessing they mean `name1` as a reference to a stateful object, i.e. not a function /Users/jgp/Music/Organized Music from Freedman/Treesdefinition...

* `except:`

Be careful when using `except:` as it will catch any sort of error. So, while you may think that only one particular error will be caught by `except:`, this is _not_ a safe assumption to make. To be safe, you should explicitly declare which exceptions to catch like so:

~~~~
try:
	foo = opne('filename')
except IOError:
	sys.exit('invalid file name')
~~~~

This will produce a `NameError` instead of falsely appearing as an `IOError`. Aside from 3rd party library which can defined exceptions that do not inherit from `Exception` you can catch `Exception` which is what the "normal" exceptions inherit from. 

* **Exceptions**

Raise them when something unexpected occurs; catch them when you can do something about it.

It's important to consider how an when errors may occur in your program. For example:

~~~~
def get_status(file):
    if not os.path.exists(file):
        print "file not found"
        sys.exit(1)
    return open(file).readline()
~~~~

If the file is deleted in between calls to `os.path.exists()` and `open()` an uncaught `IOError` exception will be thrown. To abate this, use exceptions coherently:

~~~~
def get_status(file):
    try:
        return open(file).readline()
    except EnvironmentError as err:
        print "Unable to open file: {}".format(err)
        sys.exit(1)
~~~~

This way, either the file is read or it fails. 