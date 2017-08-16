# Binary Ninja plugin development trick

## Introduction

[Binary Ninja](https://binary.ninja) is a interesting reverse engineering platform developed by [Vector 35](https://vector35.com/). One of the key features of Binary Ninja is that most of the functionality is accessible via a [Python API](https://api.binary.ninja/). Binary Ninja can be purchased in two versions "Personal" and "Commercial". This post shows a trick that helps developing Binary Ninja plugins using the personal version. The commercial version is able to work even headless without the GUI but this feature is missing from the personal version. To make things worse as Binary Ninja is still pretty new the embedded Python console is missing tab completion.

## Accessing Binary Ninja using a remote debugger

To get tab completion working and to debug crashed plugins in place we are going to use the [Embedded Python Debugger](https://github.com/sassoftware/epdb). After installation via `pip install epdb` it is possible to just use `import epdb; epdb.serve()` inside the Binary Ninja console and `import epdb; epdb.connect()` inside a normal python console outside Binary Ninja to connect the two consoles. 

[![demo](https://asciinema.org/a/RpQNyLiuV0ODeEhGDPzFsWRBU.png)](https://asciinema.org/a/RpQNyLiuV0ODeEhGDPzFsWRBU)

## Debugging

To ease debugging of newly developed Binary Ninja scripts one could `epdb.serve()` in a exception handler.

```python
from binaryninja import *
import epdb

def test(context):
    try:
    	... do something ...
        
    except Exception as e:
        epdb.serve()
        
PluginCommand.register("Testing some stuff", "", test)

```
