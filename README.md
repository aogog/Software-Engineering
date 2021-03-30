# Order  
## 1. Tables  
## 2. Task list items  
## 3. Strikethrough   
## 4. Autolinks  
## 5. Disallowed Raw HTML  
***  

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

  The header row must match the delimiter row in the number of cells. If not, a table will not be recognized  
### Ex)  
```
| abc | def |
| --- |
| bar |
```  
### Result :  
| abc | def |
| --- |
| bar |  

  The remainder of the table’s rows may vary in the number of cells. If there are a number of cells fewer than the number of cells in the header row, empty cells are inserted. If there are greater, the excess is ignored  
### Ex)  
```
| abc | def |
| --- | --- |
| bar |
| bar | baz | boo |
```  
### Result :  
| abc | def |
| --- | --- |
| bar |
| bar | baz | boo |  

  If there are no rows in the body, no `<tbody>` is generated in HTML output  
### Ex)  
``` 
| abc | def |
| --- | --- |
```  
### Result :  
| abc | def |
| --- | --- |  

***  
# 2. Task list items  
  GFM enables the tasklist extension, where an additional processing step is performed on list items.  
  A task list item is a list item where the first block in it is a paragraph which begins with a task list item marker and at least one whitespace character before any other content.  
  A task list item marker consists of an optional number of spaces, a left bracket ([), either a whitespace character or the letter x in either lowercase or uppercase, and then a right bracket (]).  
  When rendered, the task list item marker is replaced with a semantic checkbox element; in an HTML output, this would be an <input type="checkbox"> element.  
  If the character between the brackets is a whitespace character, the checkbox is unchecked. Otherwise, the checkbox is checked.  
  This spec does not define how the checkbox elements are interacted with: in practice, implementors are free to render the checkboxes as disabled or inmutable elements, or they may dynamically handle dynamic interactions (i.e. checking, unchecking) in the final rendered document.  
### Ex)  
```
- [ ] foo
- [x] bar
```  
### Result :  
- [ ] foo
- [x] bar  
#  
 Task lists can be arbitrarily nested
### Ex)  
```
- [x] foo
  - [ ] bar
  - [x] baz
- [ ] bim
```  
### Result :  
- [x] foo
  - [ ] bar
  - [x] baz
- [ ] bim  
***  
# 3. Strikethrough  
  GFM enables the strikethrough extension, where an additional emphasis type is available.  
  Strikethrough text is any text wrapped in two tildes (~).  
### Ex)  
```
~~Hi~~ Hello, world!
```  
### Result :  
~~Hi~~ Hello, world!  
#  
  As with regular emphasis delimiters, a new paragraph will cause strikethrough parsing to cease.  
### Ex)  
```
This ~~has a

new paragraph~~.
```  
### Result :  
This ~~has a

new paragraph~~.    
***  

# 4. Autolinks  
  GFM enables the autolink extension, where autolinks will be recognised in a greater number of conditions.  
  Autolinks can also be constructed without requiring the use of \< and to \> to delimit them, although they will be recognized under a smaller set of circumstances. All such recognized autolinks can only come at the beginning of a line, after whitespace, or any of the delimiting characters \*, \_, \~, and \(.  
  An extended www autolink will be recognized when the text www. is found followed by a valid domain. A valid domain consists of segments of alphanumeric characters, underscores (\_) and hyphens (\-) separated by periods (\.). There must be at least one period, and no underscores may be present in the last two segments of the domain.  
  The scheme http will be inserted automatically  
### Ex)  
```
www.commonmark.org
```  
### Result :  
www.commonmark.org  

  After a valid domain, zero or more non-space non\-< characters may follow  
### Ex)  
```
Visit www.commonmark.org/help for more information.
```  
### Result :  
Visit www.commonmark.org/help for more information.  

  We then apply extended autolink path validation as follows  
  Trailing punctuation \(specifically, ?, !, ., ,, :, \*, \_, and \~) will not be considered part of the autolink, though they may be included in the interior of the link:  
### Ex)  
```
Visit www.commonmark.org.

Visit www.commonmark.org/a.b.
```  
### Result :  
Visit www.commonmark.org.  

Visit www.commonmark.org/a.b.  
  
  When an autolink ends in \), we scan the entire autolink for the total number of parentheses. If there is a greater number of closing parentheses than opening ones, we don’t consider the unmatched trailing parentheses part of the autolink, in order to facilitate including an autolink inside a parenthesis:  
### Ex)  
```
www.google.com/search?q=Markup+(business)

www.google.com/search?q=Markup+(business)))

(www.google.com/search?q=Markup+(business))

(www.google.com/search?q=Markup+(business)
```  
### Result :  
www.google.com/search?q=Markup+(business)  

www.google.com/search?q=Markup+(business)))  

(www.google.com/search?q=Markup+(business))  

(www.google.com/search?q=Markup+(business)  

  This check is only done when the link ends in a closing parentheses ), so if the only parentheses are in the interior of the autolink, no special rules are applied:  
### Ex)  
```
www.google.com/search?q=(business))+ok
```  
### Result :  
www.google.com/search?q=(business))+ok  

  If an autolink ends in a semicolon (;), we check to see if it appears to resemble an entity reference; if the preceding text is & followed by one or more alphanumeric characters. If so, it is excluded from the autolink  
### Ex)  
```
www.google.com/search?q=commonmark&hl=en

www.google.com/search?q=commonmark&hl;
```  
### Result :  
www.google.com/search?q=commonmark&hl=en  

www.google.com/search?q=commonmark&hl;  

  \< immediately ends an autolink  
### Ex)  
```
www.commonmark.org/he<lp
```  
### Result :  
www.commonmark.org/he<lp  

  An extended url autolink will be recognised when one of the schemes http://, or https://, followed by a valid domain, then zero or more non-space non-< characters according to extended autolink path validation  
### Ex)  
```
http://commonmark.org

(Visit https://encrypted.google.com/search?q=Markup+(business))
```  
### Result :  
http://commonmark.org  

(Visit https://encrypted.google.com/search?q=Markup+(business))  

  An extended email autolink will be recognised when an email address is recognised within any text node. Email addresses are recognised according to the following rules  
  - `One ore more characters which are alphanumeric, or ., -, _, or +.`  
  - `An @ symbol.`  
  - `One or more characters which are alphanumeric, or - or _, separated by periods (.). There must be at least one period. The last character must not be one of - or _.`  
  The scheme mailto: will automatically be added to the generated link  
### Ex)  
```
foo@bar.baz
```  
### Result :  
foo@bar.baz  

  \+ can occur before the @, but not after.
### Ex)  
```
hello@mail+xyz.example isn't valid, but hello+xyz@mail.example is.
```  
### Result :  
hello@mail+xyz.example isn't valid, but hello+xyz@mail.example is.  

  \., -, and _ can occur on both sides of the @, but only . may occur at the end of the email address, in which case it will not be considered part of the address  
### Ex)  
```
a.b-c_d@a.b

a.b-c_d@a.b.

a.b-c_d@a.b-

a.b-c_d@a.b_
```  
### Result :  
a.b-c_d@a.b  

a.b-c_d@a.b.  

a.b-c_d@a.b-  

a.b-c_d@a.b_  
***  
# 5. Disallowed Raw HTML  
  GFM enables the tagfilter extension, where the following HTML tags will be filtered when rendering HTML output:  
   - `<title>`  
   - `<textarea>`  
   - `<style>`  
   - `<xmp>`  
   - `<iframe>`  
   - `<noembed>`  
   - `<noframes>`  
   - `<script>`  
   - `<plaintext>`  
  
  Filtering is done by replacing the leading \< with the entity &lt;. These tags are chosen in particular as they change how HTML is interpreted in a way unique to them (i.e. nested HTML is interpreted differently), and this is usually undesireable in the context of other rendered Markdown content.  
  All other HTML tags are left untouched.  
### Ex)  
```
<strong> <title> <style> <em>

<blockquote>
  <xmp> is disallowed.  <XMP> is also disallowed.
</blockquote>
```  
### Result :  
<strong> <title> <style> <em>  
  
<blockquote>  
  <xmp> is disallowed.  <XMP> is also disallowed.  
</blockquote>  
***
  
