## json文件
简单地说，JSON 可以将 JavaScript 对象中表示的一组数据转换为字符串，然后就可以在网络或者程序之间轻松地传递这个字符串，并在需要的时候将它还原为各编程语言所支持的数据格式，例如在 PHP 中，可以将 JSON 还原为数组或者一个基本对象。在用到AJAX时，如果需要用到数组传值，这时就需要用JSON将数组转化为字符串。

### 表示对象

对象是一个无序的“‘名称/值’对”集合。一个对象以{左括号开始，}右括号结束。每个“名称”后跟一个:冒号；“‘名称/值’ 对”之间使用,逗号分隔。  bool类型与数值类型可以不加双引号
                              
``` json
{
    "glossary": {
        "title": "example glossary",
		"GlossDiv": {
            "title": "S",
			"GlossList": {
                "GlossEntry": {
                    "ID": "SGML",
					"SortAs": "SGML",
					"GlossTerm": "Standard Generalized Markup Language",
					"Acronym": "SGML",
					"Abbrev": "ISO 8879:1986",
					"GlossDef": {
                        "para": "A meta-markup language, used to create markup languages such as DocBook.",
						"GlossSeeAlso": ["GML", "XML"]
                    },
					"GlossSee": "markup"
                }
            }
        }
    }
}
```
### 表示数组

和普通的 JS 数组一样，JSON 表示数组的方式也是使用方括号 []。



## yaml文件

- 不用方括号  改为使用缩进(python风格)
- 不用加双引号, 遇到数值和bool类型的时候会自动转换(当然解析的代价会高于json)
- 数组类型用 `-` 表示而不是  `[]`  框选

```yaml
---
 doe: "a deer, a female deer"
 ray: "a drop of golden sun"
 pi: 3.14159
 xmas: true
 french-hens: 3
 calling-birds:
   - huey
   - dewey
   - louie
   - fred
 xmas-fifth-day:
   calling-birds: four
   french-hens: 3
   golden-rings: 5
   partridges:
     count: 1
     location: "a pear tree"
   turtle-doves: two
```