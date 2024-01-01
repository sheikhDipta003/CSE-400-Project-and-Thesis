This Python script contains utility functions typically used for handling JSON files and processing statistical data. It's likely designed to assist in operations related to AWS Lambda functions, particularly for profiling and monitoring purposes. Here's a detailed breakdown of each part of the script:

### Global Variables
```python
PROFILER_APPDIR = "/mnt/DATA1/ajayn/shm/profiler/faastlane/composer/profiler"
PROFILER_FUNC_NAME = "main"
PROFILER_HANDLER = "handler"
```
These are constants defining the directory of the profiler application (`PROFILER_APPDIR`), the name of the main function file (`PROFILER_FUNC_NAME`), and the handler function (`PROFILER_HANDLER`). They seem to be specific to a profiler tool used for AWS Lambda functions.

### `getJson` Function
```python
def getJson(fileName):
    content = None
    try:
        f = open(fileName, "r")
        content = json.loads(f.read())
        f.close()
    except Exception as e:
        pass
    return content
```
This function reads a JSON file and returns its content as a Python dictionary.

1. `def getJson(fileName):` - Function definition with `fileName` as the parameter.
2. `content = None` - Initializes `content` to `None`. This acts as the default return value if reading the file fails.
3. `try:` - Begins a try block to catch exceptions.
4. `f = open(fileName, "r")` - Opens the file in read mode.
5. `content = json.loads(f.read())` - Reads the file content and parses the JSON string into a Python dictionary.
6. `f.close()` - Closes the file.
7. `except Exception as e:` - Catches any exceptions that occur during file reading or JSON parsing.
8. `pass` - Does nothing in case of an exception, effectively ignoring errors.
9. `return content` - Returns the parsed content or `None` if an exception occurred.

### `putJson` Function
```python
def putJson(fileName, content):
    f = open(fileName, "w")
    f.write(json.dumps(content))
    f.close()
```
This function writes a Python dictionary to a file in JSON format.

1. `def putJson(fileName, content):` - Function definition with `fileName` (where to write) and `content` (what to write) as parameters.
2. `f = open(fileName, "w")` - Opens the file in write mode.
3. `f.write(json.dumps(content))` - Converts `content` to a JSON string and writes it to the file.
4. `f.close()` - Closes the file after writing.

### `getStats` Function
```python
def getStats(data):
    data.sort()
    items = len(data)
    median = data[int(0.5*items)]
    later = data[int(0.99*items)]
    return (median, later)
```
This function calculates and returns the median and 99th percentile values from a list of numeric data.

1. `def getStats(data):` - Function definition with `data` (a list of numbers) as the parameter.
2. `data.sort()` - Sorts the data in ascending order.
3. `items = len(data)` - Stores the total number of items in `data`.
4. `median = data[int(0.5*items)]` - Finds the median (middle value) of the sorted list.
5. `later = data[int(0.99*items)]` - Finds the 99th percentile value (the value below which 99% of the data fall).
6. `return (median, later)` - Returns a tuple containing the median and 99th percentile values.

### Overall Context
These functions are utility tools for handling JSON data and basic statistical analysis. They are likely used in the context of analyzing performance metrics of AWS Lambda functions, as indicated by the constants defined at the beginning of the script.