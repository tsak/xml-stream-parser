## xml stream parser 
xml-stream-parser is xml parser for GO. It is efficient to parse large xml data with streaming fashion. 

### Usage

```xml
<?xml version="1.0" encoding="UTF-8"?>
<bookstore>
   <book>
      <title>The Iliad and The Odyssey</title>
      <price>12.95</price>
      <comments>
         <userComment rating="4">Best translation I've read.</userComment>
         <userComment rating="2">I like other versions better.</userComment>
      </comments>
   </book>
   <book>
      <title>Anthology of World Literature</title>
      <price>24.95</price>
      <comments>
         <userComment rating="3">Needs more modern literature.</userComment>
         <userComment rating="4">Excellent overview of world literature.</userComment>
      </comments>
   </book>
</bookstore>
```

<b>Stream</b> over books
```go
f, _ := os.Open("input.xml")
br := bufio.NewReaderSize(f,8192)
parser := pr.NewXmlParser(br, "books")

for xml := range *parser.Stream() {
	fmt.Println(xml.Childs["title"][0].InnerText)
	fmt.Println(xml.Childs["comments"][0].Childs["userComment"][0].Attrs["rating"])
	fmt.Println(xml.Childs["comments"][0].Childs["userComment"][0].InnerText)
}
   
```

<b>Skip</b> tags for speed
```go
parser := pr.NewXmlParser(br, "books").SkipElements([]string{"price", "comments"})
```

<b>Error</b> handlings
```go
for xml := range *parser.Stream() {
   if xml.Err !=nil { 
      // handle error
   }
}
```

<b>Progress</b> of parsing
```go
// total byte read to calculate the progress of parsing
parser.TotalReadSize
```




If you interested check also [json parser](https://github.com/tamerh/jsparser) which works similarly
