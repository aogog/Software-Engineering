# Extended Syntax  
## 1. Tables  
  GFM enables the table extension, where an additional leaf block type is available.  
  A table is an arrangement of data with rows and columns, consisting of a single header row, a delimiter row separating the header from the data, and zero or more data rows.  
  Each row consists of cells containing arbitrary text, in which inlines are parsed, separated by pipes (|). A leading and trailing pipe is also recommended for clarity of reading, and if there’s otherwise parsing ambiguity. Spaces between pipes and cell content are trimmed. Block-level elements cannot be inserted in a table.  
  The delimiter row consists of cells whose only content are hyphens (-), and optionally, a leading or trailing colon (:), or both, to indicate left, right, or center alignment respectively.  
### Ex)  
```
| foo | bar |
| --- | --- |
| baz | bim |
```  
### Result :  
| foo | bar |
| --- | --- |
| baz | bim |  

  Cells in one column don’t need to match length, though it’s easier to read if they are. Likewise, use of leading and trailing pipes may be inconsistent:  
### Ex)  
```
| abc | defghi |
:-: | -----------:
bar | baz
```  
### Result  :  
| abc | defghi |
:-: | -----------:
bar | baz  

  Include a pipe in a cell’s content by escaping it, including inside other inline spans:  
### Ex)  
```
| f\|oo  |
| ------ |
| b `\|` az |
| b **\|** im |
```  
### Result :  
| f\|oo  |
| ------ |
| b `\|` az |
| b **\|** im |  

  The table is broken at the first empty line, or beginning of another block-level structure:  
### Ex 1)  
```
| abc | def |
| --- | --- |
| bar | baz |
> bar
```  
### Result :  
| abc | def |
| --- | --- |
| bar | baz |
> bar  
### Ex 2)  
```
| abc | def |
| --- | --- |
| bar | baz |
bar

bar  
```  
### Result :  
| abc | def |
| --- | --- |
| bar | baz |
bar

bar  
