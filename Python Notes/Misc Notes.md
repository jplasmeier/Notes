# Python Notes

## Miscellaneous Notes

Anything that I learn about Python that doesn't fit within its own notes document or anywhere else.

### Custom Decorators

Definition:
From *High Performance Python* by Gorelick and Oszvald:

```
from functools import wraps

def timefn(fn):
    @wraps(fn)
    def measure_time(*args, **kwargs):
        t1 = time.time()
        result = fn(*args, **kwargs)
        t2 = time.time()
        print ("@timefn:" + fn.func_name + " took " + str(t2 - t1) + " seconds.")
        return result
    return measure_time
```

Now, when you put `@timefn` over a function declaration, this will calculate the time it took to run that function.

### Unix `time` and Page Faults

This isn't even specific to Python, but you can run `time --verbose python foo.py` to get a count of `Major (requiring I/O) page faults` which tells you when your data is being dumped to disk from RAM (causing performance loss). It will also give you an idea of how much time your program is spent in the CPU vs waiting for an external response (I/O). 

### autoenv

You can configure your OS to automatically activate a virtual environment like so:

In bash:

```
deactivate
pip install autoenv==1.0.0
touch .env
```
In that new file `.env`:

```
source bin/activate
```

In bash again:

```
echo "source `which activate.sh`" >> ~/.bashrc
source ~/.bashrc
```

I don't know of a way to deactivate on leaving the directory, though.

### Directories

Getting the current directory (relative or absolute) can be done in a few ways:

dirname: `os.path.dirname(path)` returns the directory portion of a given path.

basename: `os.path.basename(filename)` returns the base name portion of a given path.

both: `os.path.dirname(filename) + os.path.basename(filename) == filename` 

So, these don't actually do all that much aside from split a given path. 

curdir: `os.path.curdir` is just a string of the current directory. This will always be `.` on UNIX systems. Kind of useless I guess.

getcwd: `os.getcwd()` will return a string of the current working directory.



