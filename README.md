# ![JSONNI](https://github.com/hannupekka/jsonni/blob/master/resources/icons/64x64.png?raw=true) JSONNI

[![Sponsored](https://img.shields.io/badge/chilicorn-sponsored-brightgreen.svg?logo=data%3Aimage%2Fpng%3Bbase64%2CiVBORw0KGgoAAAANSUhEUgAAAA4AAAAPCAMAAADjyg5GAAABqlBMVEUAAAAzmTM3pEn%2FSTGhVSY4ZD43STdOXk5lSGAyhz41iz8xkz2HUCWFFhTFFRUzZDvbIB00Zzoyfj9zlHY0ZzmMfY0ydT0zjj92l3qjeR3dNSkoZp4ykEAzjT8ylUBlgj0yiT0ymECkwKjWqAyjuqcghpUykD%2BUQCKoQyAHb%2BgylkAyl0EynkEzmkA0mUA3mj86oUg7oUo8n0k%2FS%2Bw%2Fo0xBnE5BpU9Br0ZKo1ZLmFZOjEhesGljuzllqW50tH14aS14qm17mX9%2Bx4GAgUCEx02JySqOvpSXvI%2BYvp2orqmpzeGrQh%2Bsr6yssa2ttK6v0bKxMBy01bm4zLu5yry7yb29x77BzMPCxsLEzMXFxsXGx8fI3PLJ08vKysrKy8rL2s3MzczOH8LR0dHW19bX19fZ2dna2trc3Nzd3d3d3t3f39%2FgtZTg4ODi4uLj4%2BPlGxLl5eXm5ubnRzPn5%2Bfo6Ojp6enqfmzq6urr6%2Bvt7e3t7u3uDwvugwbu7u7v6Obv8fDz8%2FP09PT2igP29vb4%2BPj6y376%2Bu%2F7%2Bfv9%2Ff39%2Fv3%2BkAH%2FAwf%2FtwD%2F9wCyh1KfAAAAKXRSTlMABQ4VGykqLjVCTVNgdXuHj5Kaq62vt77ExNPX2%2Bju8vX6%2Bvr7%2FP7%2B%2FiiUMfUAAADTSURBVAjXBcFRTsIwHAfgX%2FtvOyjdYDUsRkFjTIwkPvjiOTyX9%2FAIJt7BF570BopEdHOOstHS%2BX0s439RGwnfuB5gSFOZAgDqjQOBivtGkCc7j%2B2e8XNzefWSu%2BsZUD1QfoTq0y6mZsUSvIkRoGYnHu6Yc63pDCjiSNE2kYLdCUAWVmK4zsxzO%2BQQFxNs5b479NHXopkbWX9U3PAwWAVSY%2FpZf1udQ7rfUpQ1CzurDPpwo16Ff2cMWjuFHX9qCV0Y0Ok4Jvh63IABUNnktl%2B6sgP%2BARIxSrT%2FMhLlAAAAAElFTkSuQmCC)](http://spiceprogram.org/oss-sponsorship)

Manipulate data with ES6, [Lodash](https://lodash.com/) or [fromfrom](https://github.com/tomi/fromfrom), on command line.

Command line version of [JSONNI](https://github.com/hannupekka/jsonni)

## Install

`npm install -g jsonni-cli`

## Features

- Reads data from `stdin`
- Supports different syntaxes for querying data
  - ES
  - Lodash
  - fromfrom
- Supports multiple output formats
  - JSON
  - CSV
  - TSV
- Customizable output indentation
- Optionally minimized output JSON

## Usage

```
Usage: jsonni [options]

Options:
  -v, --version                   output the version number
  -m --minify                     minify output
  -q --query <query>              query to transform data with
  --input <format>                input format. Available formats: json, tsv, csv
  --input-header <header>         input CSV/TSV headers. Separated with ","
  --input-delimiter <delimiter>   CSV/TSV input delimiter character. Defaults to ","
  --output <format>               output format. Available formats: json, tsv, csv
  --output-delimiter <delimiter>  CSV/TSV output delimiter character. Defaults to ","
  --indent <indent>               output indentation, defaults to "  "
  -h, --help                      output usage information
```

### About `-q` parameter

Parameter value should be quoted with single quotes (`'`) instead of double (`"`).

## Examples

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni
```

![default.png](./screenshots/default.png)

### Grab first item

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q '$input[0]'
```

![json_es_first.png](./screenshots/json_es_first.png)

### Filter

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q '$input.filter(item => item.completed)'
```

![json_es_filter.png](./screenshots/json_es_filter.png)

### Lodash, with chain

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q '_.chain($input).orderBy(["title"], ["asc"]).map("title")'
```

![json_lodash_chain.png](./screenshots/json_lodash_chain.png)

### Lodash, without chain

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q '_.map(_.orderBy($input, ["title"], ["asc"]), "title")'
```

![json_lodash.png](./screenshots/json_lodash.png)

### fromfrom

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q 'from($input).filter(item => item.title.startsWith("fugiat")).toArray()'
```

![fromfrom.png](./screenshots/fromfrom.png)

### JSON, with custom indentation

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q '$input[0]' --indent="\t"
```

![json_custom_indent.png](./screenshots/json_custom_indent.png)

### JSON to CSV

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q '$input[0]' --output=csv --output-delimiter=";"
```

![json_to_csv.png](./screenshots/json_to_csv.png)

### JSON to TSV

```
curl --silent http://jsonplaceholder.typicode.com/todos?_limit=5 | jsonni -q '$input[0]' --output=tsv
```

![json_to_tsv.png](./screenshots/json_to_tsv.png)

### CSV to JSON

```
cat csv.csv | jsonni -q '$input[0]' --input=csv
```

![csv_to_json.png](./screenshots/csv_to_json.png)

### CSV to CSV

```
cat csv.csv | jsonni -q '$input.filter(user => user.isActive)' --input=csv --output=csv
```

![csv_to_csv.png](./screenshots/csv_to_csv.png)

### CSV to TSV

```
cat csv.csv | jsonni -q '$input.filter(user => user.isActive)' --input=csv --output=tsv
```

![csv_to_tsv.png](./screenshots/csv_to_tsv.png)

### CSV without headers

```
cat csvWithoutHeaders.csv | jsonni -q '$input[0]' --input=csv --input-header=id,isActive,age,name,registered
```

![csv_without_headers.png](./screenshots/csv_without_headers.png)

## Acknowledgements

This project is a grateful recipient of the [Futurice Open Source sponsorship program](http://futurice.com/blog/sponsoring-free-time-open-source-activities).

Icon made by [Freepik](https://www.flaticon.com/authors/freepik) from [www.flaticon.com](https://www.flaticon.com)

## License

MIT
