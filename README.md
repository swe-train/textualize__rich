[![PyPI version](https://badge.fury.io/py/rich.svg)](https://badge.fury.io/py/rich)
[![PyPI](https://img.shields.io/pypi/pyversions/rich.svg)](https://pypi.org/project/rich/)
[![Downloads](https://pepy.tech/badge/rich/month)](https://pepy.tech/project/rich/month)
[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://awesome-python.com/#command-line-interface-development)

[![Rich blog](https://img.shields.io/badge/Blog-rich%20news-yellowgreen)](https://www.willmcgugan.com/tag/rich/)
[![Twitter Follow](https://img.shields.io/twitter/follow/willmcgugan.svg?style=social)](https://twitter.com/willmcgugan)

# Rich

Rich is a Python library for rendering _rich_ text and beautiful formatting to the terminal.

The [Rich API](https://rich.readthedocs.io/en/latest/) makes it easy to add colorful text (up to 16.7 million colors) with styles (bold, italic, underline etc.) to your script or application. Rich can also render pretty tables, progress bars, markdown, syntax highlighted source code, and tracebacks -- out of the box.

![Features](https://github.com/willmcgugan/rich/raw/master/imgs/features.png)

## Compatibility

Rich works with Linux, OSX, and Windows. True color / emoji works with new Windows Terminal, classic terminal is limited to 8 colors.

## Installing

Install with `pip` or your favorite PyPi package manager.

```
pip install rich
```

## Rich print function

To effortlessly add rich output to your application, you can import the [rich print](https://rich.readthedocs.io/en/latest/introduction.html#quick-start) method, which has the same signature as the builtin Python function. Try this:

```python
from rich import print

print("Hello, [bold magenta]World[/bold magenta]!", ":vampire:", locals())
```

![Hello World](https://github.com/willmcgugan/rich/raw/master/imgs/print.png)

## Using the Console

For more control over rich terminal content, import and construct a [Console](https://rich.readthedocs.io/en/latest/reference/console.html#rich.console.Console) object.

```python
from rich.console import Console

console = Console()
```

The Console object has a `print` method which has an intentionally similar interface to the builtin `print` function. Here's an example of use:

```python
console.print("Hello", "World!")
```

As you might expect, this will print `"Hello World!"` to the terminal. Note that unlike the builtin `print` function, Rich will word-wrap your text to fit within the terminal width.

There are a few ways of adding color and style to your output. You can set a style for the entire output by adding a `style` keyword argument. Here's an example:

```python
console.print("Hello", "World!", style="bold red")
```

The output will be something like the following:

![Hello World](https://github.com/willmcgugan/rich/raw/master/imgs/hello_world.png)

That's fine for styling a line of text at a time. For more finely grained styling, Rich renders a special markup which is similar in syntax to [bbcode](https://en.wikipedia.org/wiki/BBCode). Here's an example:

```python
console.print("Where there is a [bold cyan]Will[/bold cyan] there [u]is[/u] a [i]way[/i].")
```

![Console Markup](https://github.com/willmcgugan/rich/raw/master/imgs/where_there_is_a_will.png)

### Console logging

The Console object has a `log()` method which has a similar interface to `print()`, but also renders a column for the current time and the file and line which made the call. By default Rich will do syntax highlighting for Python structures and for repr strings. If you log a collection (i.e. a dict or a list) Rich will pretty print it so that it fits in the available space. Here's an example of some of these features.

```python
from rich.console import Console
console = Console()

test_data = [
    {"jsonrpc": "2.0", "method": "sum", "params": [None, 1, 2, 4, False, True], "id": "1",},
    {"jsonrpc": "2.0", "method": "notify_hello", "params": [7]},
    {"jsonrpc": "2.0", "method": "subtract", "params": [42, 23], "id": "2"},
]

def test_log():
    enabled = False
    context = {
        "foo": "bar",
    }
    movies = ["Deadpool", "Rise of the Skywalker"]
    console.log("Hello from", console, "!")
    console.log(test_data, log_locals=True)


test_log()
```

The above produces the following output:

![Log](https://github.com/willmcgugan/rich/raw/master/imgs/log.png)

Note the `log_locals` argument, which outputs a table containing the local variables where the log method was called.

The log method could be used for logging to the terminal for long running applications such as servers, but is also a very nice debugging aid.

### Logging Handler

You can also use the builtin [Handler class](https://rich.readthedocs.io/en/latest/logging.html) to format and colorize output from Python's logging module. Here's an example of the output:

![Logging](https://github.com/willmcgugan/rich/blob/master/imgs/logging.png)

## Emoji

To insert an emoji in to console output place the name between two colons. Here's an example:

```python
>>> console.print(":smiley: :vampire: :pile_of_poo: :thumbs_up: :raccoon:")
😃 🧛 💩 👍 🦝
```

Please use this feature wisely.

## Progress Bars

Rich can render multiple flicker-free [progress](https://rich.readthedocs.io/en/latest/progress.html) bars to track long-running tasks.

For basic usage, wrap any sequence in the `track` function and iterate over the result. Here's an example:

```python
from rich.progress import track

for step in track(range(100)):
    do_step(step)
```

It's not much harder to add multiple progress bars. Here's an example taken from the docs:

![progress](https://github.com/willmcgugan/rich/raw/master/imgs/progress.gif)

The columns may be configured to show any details you want. Built-in columns include percentage complete, file size, file speed, and time remaining. Here's another example showing a download in progress:

![progress](https://github.com/willmcgugan/rich/raw/master/imgs/downloader.gif)

To try this out yourself, see [examples/downloader.py](https://github.com/willmcgugan/rich/blob/master/examples/downloader.py) which can download multiple URLs simultaneously while displaying progress.

## Markdown

Rich can render [markdown](https://rich.readthedocs.io/en/latest/markdown.html) and does a reasonable job of translating the formatting to the terminal.

To render markdown import the `Markdown` class and construct it with a string containing markdown code. Then print it to the console. Here's an example:

```python
from rich.console import Console
from rich.markdown import Markdown

console = Console()
with open("README.md") as readme:
    markdown = Markdown(readme.read())
console.print(markdown)
```

This will produce output something like the following:

![markdown](https://github.com/willmcgugan/rich/raw/master/imgs/markdown.png)

## Syntax Highlighting

Rich uses the [pygments](https://pygments.org/) library to implement [syntax highlighting](https://rich.readthedocs.io/en/latest/syntax.html). Usage is similar to rendering markdown; construct a `Syntax` object and print it to the console. Here's an example:

```python
from rich.console import Console
from rich.syntax import Syntax

my_code = '''
def iter_first_last(values: Iterable[T]) -> Iterable[Tuple[bool, bool, T]]:
    """Iterate and generate a tuple with a flag for first and last value."""
    iter_values = iter(values)
    try:
        previous_value = next(iter_values)
    except StopIteration:
        return
    first = True
    for value in iter_values:
        yield first, False, previous_value
        first = False
        previous_value = value
    yield first, True, previous_value
'''
syntax = Syntax(my_code, "python", theme="monokai", line_numbers=True)
console = Console()
console.print(syntax)
```

This will produce the following output:

![syntax](https://github.com/willmcgugan/rich/raw/master/imgs/syntax.png)

## Tables

Rich can render flexible tables with unicode box characters. There is a large variety of formatting options for borders, styles, cell alignment etc. Here's a simple example:

```python
from rich.console import Console
from rich.table import Column, Table

console = Console()

table = Table(show_header=True, header_style="bold magenta")
table.add_column("Date", style="dim", width=12)
table.add_column("Title")
table.add_column("Production Budget", justify="right")
table.add_column("Box Office", justify="right")
table.add_row(
    "Dev 20, 2019", "Star Wars: The Rise of Skywalker", "$275,000,000", "$375,126,118"
)
table.add_row(
    "May 25, 2018",
    "[red]Solo[/red]: A Star Wars Story",
    "$275,000,000",
    "$393,151,347",
)
table.add_row(
    "Dec 15, 2017",
    "Star Wars Ep. VIII: The Last Jedi",
    "$262,000,000",
    "[bold]$1,332,539,889[/bold]",
)

console.print(table)
```

This produces the following output:

![table](https://github.com/willmcgugan/rich/raw/master/imgs/table.png)

Note that console markup is rendered in the same was as `print()` and `log()`. In fact, anything that is renderable by Rich may be included in the headers / rows (even other tables).

The `Table` class is smart enough to resize columns to fit the available width of the terminal, wrapping text as required. Here's the same example, with the terminal made smaller than the table above:

![table2](https://github.com/willmcgugan/rich/raw/master/imgs/table2.png)

## Tracebacks

Rich can render beautiful tracebacks which are easier to read and show more code than standard Python tracebacks. You can set Rich as the default traceback handler so all uncaught exceptions will be rendered by Rich.

Here's what it looks like on OSX (similar on Linux):

![traceback](https://github.com/willmcgugan/rich/raw/master/imgs/traceback.png)

Here's what it looks like on Windows:

![traceback_windows](https://github.com/willmcgugan/rich/raw/master/imgs/traceback_windows.png)

See the [rich traceback](https://rich.readthedocs.io/en/latest/traceback.html) documentation for the details.
