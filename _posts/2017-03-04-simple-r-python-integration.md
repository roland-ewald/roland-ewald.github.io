---
title:  "A (very) simple R -> Python integration"
date:   2017-03-04
tags: R python programming integration simplicity
---

For a side project, I needed to run a piece of Python code from R. Given their popularity (e.g., they hold the spots \#2 and \#9 in [the PYPL popularity of programming languages index](https://pypl.github.io/PYPL.html)) and their focus on being pragmatic, I thought this should be very simple.

I was wrong. There seems to be an easy solution to call R from Python (via [rpy](https://rpy2.bitbucket.io/)) but the other way around turns out to be much more difficult. The packages that promise to do so (such as [rPython](https://cran.r-project.org/web/packages/rPython/index.html) or [rPythonWin](https://github.com/cjgb/rPython-win)) seem to be not really in widespread use, and are thus not maintained very well[^1].

I shouldn't complain about open source (and send patches instead ;-), but there seems to be no well-maintained, easy-to-use, cross-platform R plugin that allows to call Python. Since I only have to make a few calls to a single Python function and runtime performance is not an issue, I gave up and simply used JSON and the command line for data transfer and invocation.

Here are some code samples that should get you going, if you also just want to cobble something together without being particularly proficient in either language.

The R snippet (requires an `install.packages("rjson")` in the R console):

{% highlight R %}
library(rjson)

python_caller <- function(x, y, z) {
    INPUT_FILE <- 'python_input.json'
    OUTPUT_FILE <- 'python_output.json'
    PYTHON_FILE <- 'my_code.py'

    # Write input parameters to JSON file
    json <- toJSON(list(
        x = x,
        y = y,
        z = z))
    fileConn <- file(INPUT_FILE)
    writeLines(json, fileConn)
    close(fileConn)

    # Run Python code
    system(paste('python ', PYTHON_FILE))

    #Read results from JSON file
    json <- readChar(OUTPUT_FILE, file.info(OUTPUT_FILE)$size)
    result <- fromJSON(json)
    file.remove(OUTPUT_FILE)

    # Return values
    return(list(result1 = result$'result1', result2 = result$'result2'))
}
{% endhighlight %}

The Python snippet for `my_code.py`:

{% highlight python %}
import os
import json

# Read input parameters from JSON file
INPUT_FILE = 'python_input.json'
OUTPUT_FILE = 'python_output.json'
with open(INPUT_FILE) as json_file:
    my_args = json.load(json_file)
os.remove(INPUT_FILE)

# Run some actual code here instead:
output = {}
output["result1"] = my_args['x'] + my_args['y'] - my_args['z']
output["result2"] = my_args['x'] - my_args['y'] + my_args['z']

# Write results to JSON file
# Note that some Python objects (e.g. NumPy arrays) need to be converted before writing them (e.g. see http://stackoverflow.com/a/32850511/109942)
with open(OUTPUT_FILE, 'w') as f:
    json.dump(output, f)
{% endhighlight %}

When calling

{% highlight R %}
python_caller(1,2,3)
{% endhighlight %}

in the R console, the intermediate files `python_input.json` and `python_output.json` look as expected (before being deleted):

* `python_input.json`:

{% highlight json %}
{"x":1,"y":2,"z":3}
{% endhighlight %}

*  `python_output.json`:

{% highlight json %}
{"result2": 2, "result1": 0}
{% endhighlight %}

This approach is far from perfect (for example: no debug mode that keeps the intermediate files, no support for multi-threading, assumes `python` to be available on the `PATH`, python command-line options like `-O` are missing), but it is also simple enough to understand this again in a few months, it requires no extra setup, and it works cross-platform (I hope ;-).

---

[^1]: This is a euphemism for me not being able to set them up, neither on Windows nor on Linux, and neither with a current R version (3.3.2) nor an old one (2.15.1) that they should support (according to their documentation).
