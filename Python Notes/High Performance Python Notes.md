# High Performance Python Notes

Notes taken while reading High Performance Python, 1st ed. by Gorelick and Ozsvald. 

## Chapter 2: Profiling to Find Bottlenecks

#### Profiling

Profiling tells you which parts of your code is using the most CPU time/RAM space/network IO/disk IO/etc. It's a quick and easy way to spot bottlenecks. Don't start optimizing your code before you profile. Profiling should be used to prioritize optimizations. 

#### The Julia Set

We're going to start with a sub-optimal implementation of a plot of the Julia Set:

```
for z in coordinates:
	for iteration in range(maxiter):
		if abs(z) < 2.0:
			z = z * z + c
		else:
			break
```
We included a time measure in our code. But this is cumbersome.

**Custom decorator**

So we wrote a custom decorator to time functions:

```
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
**timeit**

One can also use `timeit` to get a good idea of how long a function is taking to run:

```
python2.7 -m timeit -n 5 -r 5 -s "import julia" "julia.calc_pure_python(desired_width=1000,max_iterations=300)"
```

This runs 5 trials of 5 runs each (from `-n` and `-r` flags respectively).

**Unix `time`**

Can also use Unix `time` command to tell how long the program spends in:

* `real`: the "wall" clock elapsed time
* `user`: amount of CPU time spent on your program outside of kernel functions 
* `sys`: time spent in kernal functions

Adding `user` and `sys` gives you the amount of CPU time your program took. If this is less than `real` it's probably from time spent on disk/network I/O or something. `time` can be used for any program. Can add the `--verbose` flag to get more output, most notably `Major (requiring I/O) page faults`. 

**cProfile**

Built-in profiling tool that hooks into the CPython VM. Useful for finding out which functions in your program are slow.

`python2.7 -m cProfile -s cumulative julia.py`

The `-s cumulative` option orders output by cumulative time spent in each function. You can also write the output to a file:

`python2.7 -m cProfile -o julia_profile.stats julia.py`

This gives us a `pstats` object which we can play around with in IPython like so:

```
import pstats
p = pstats.Stats("julia_profile.stats")
p.sort_stats("cumulative")
p.print_stats()
```

You can do some further analysis with `print_callers()` and `print_callees()`.

**runsnake**

Visualization tool for cProfile stats. Apparently one of its dependencies doesn't work well with `virtualenv`. Not sure how to actually run it, tbh.

**line_profiler**

While cProfile and previous approaches work well for finding slow functions, line_profiler will go through a function and tell you how long each line takes.





