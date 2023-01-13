[![Documentation](https://img.shields.io/readthedocs/yankee/stable)](https://yankee.readthedocs.io/en/stable/)


[![PyPI](https://img.shields.io/pypi/v/yankee?color=blue)](https://pypi.org/project/yankee)
[![PyPI - Python Versions](https://img.shields.io/pypi/pyversions/yankee)](https://pypi.org/project/yankee)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/yankee?color=blue)](https://pypi.org/project/yankee)


# Summary

Simple declarative data extraction and loading in Python, featuring:

- 🍰 **Ease of use:** Data extraction is performed in a simple, declarative types.
- ⚙ **XML / HTML / JSON Extraction:** Extraction can be performed across a wide array of structured data
- 🐼 **Pandas Integration:** Results are easily castable to [Pandas Dataframes and Series][pandas].
- 🚀 **Performance:** XML loading is supported by the excellent and fast [lxml] library, JSON is supported by [UltraJSON][ujson] for fast parsing, and [jsonpath_ng] for flexible data extraction.  

[lxml]: https://lxml.de/
[ujson]:https://github.com/ultrajson/ultrajson
[jsonpath_ng]: https://github.com/h2non/jsonpath-ng
[pandas]: https://pandas.pydata.org/pandas-docs/stable/

## Quick Start

To extract data from **XML**, use this import statement, and see the example below:
```python
from yankee.xml.schema import Schema, fields as f, CSSSelector
```

To extract data from **JSON**, use this import statement, and see the example below:
```python
from yankee.xml.schema import Schema, fields as f, JSONPath
```

To extract data from **HTML**, use this import statement:
```python
from yankee.html.schema import Schema, fields as f, CSSSelector
```

To extract data from **Python objects** (either objects or dictionaries), use this import statement:
```python
from yankee.base.schema import Schema, fields as f
```
<!-- RTD-IGNORE -->
## Documentation

Complete documentation is available on [Read The Docs]
[Read The Docs]: https://yankee.readthedocs.io/en/latest/

<!-- END-RTD-IGNORE -->
## Examples

### Extract data from XML

Data extraction from XML. By default, data keys are XPath expressions, but can also be CSS selectors.

Take this:
```xml
    <xmlObject>
        <name>Johnny Appleseed</name>
        <birthdate>2000-01-01</birthdate>
        <something>
            <many>
                <levels>
                    <deep>123</deep>
                </levels>
            </many>
        </something>
    </xmlObject>
```

Do this:
```python
from yankee.xml.schema import Schema, fields as f, CSSSelector

class XmlExample(Schema):
    name = f.String("./name")
    birthday = f.Date(CSSSelector("birthdate"))
    deep_data = f.Int("./something/many/levels/deep")

XmlExample().load(xml_doc)
```

Get this:
```python
{
    "name": "Johnny Appleseed",
    "birthday": datetime.date(2000, 1, 1),
    "deep_data": 123
}
```

### Extract data from JSON

Data extraction from JSON. By default, data keys are implied from the field names, but can also be JSONPath expressions

Take this:
```json
{
        "name": "Johnny Appleseed",
        "birthdate": "2000-01-01",
        "something": [
            {"many": {
                "levels": {
                    "deep": 123
                }
            }}
        ]
    }
```
Do this:
```python
from yankee.json.schema import Schema, fields as f

class JsonExample(Schema):
    name = f.String()
    birthday = f.Date("birthdate")
    deep_data = f.Int("something.0.many.levels.deep")
```
Get this:
```python
{
    "name": "Johnny Appleseed",
    "birthday": datetime.date(2000, 1, 1),
    "deep_data": 123
}
```


