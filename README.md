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
