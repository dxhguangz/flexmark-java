---
title: Autolink Extension Spec
author: Vladimir Schneider
version: 0.1
date: '2016-06-06'
license: '[CC-BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/)'
...

---

## Autolink

Autolink extension converts links and e-mail addresses to links in the markdown source.

The tests here are converted to commonmark spec.txt format and AST expected results added.

```````````````````````````````` example Autolink: 1
foo http://one.org/ bar http://two.org/
.
<p>foo <a href="http://one.org/">http://one.org/</a> bar <a href="http://two.org/">http://two.org/</a></p>
.
Document[0, 40]
  Paragraph[0, 40]
    TextBase[0, 39] chars:[0, 39, "foo h … .org/"]
      Text[0, 4] chars:[0, 4, "foo "]
      AutoLink[4, 19] text:[4, 19, "http://one.org/"] pageRef:[4, 19, "http://one.org/"]
        Text[4, 19] chars:[4, 19, "http: … .org/"]
      Text[19, 24] chars:[19, 24, " bar "]
      AutoLink[24, 39] text:[24, 39, "http://two.org/"] pageRef:[24, 39, "http://two.org/"]
        Text[24, 39] chars:[24, 39, "http: … .org/"]
````````````````````````````````


```````````````````````````````` example Autolink: 2
foo http://one.org/ bar `code` baz http://two.org/
.
<p>foo <a href="http://one.org/">http://one.org/</a> bar <code>code</code> baz <a href="http://two.org/">http://two.org/</a></p>
.
Document[0, 51]
  Paragraph[0, 51]
    TextBase[0, 24] chars:[0, 24, "foo h …  bar "]
      Text[0, 4] chars:[0, 4, "foo "]
      AutoLink[4, 19] text:[4, 19, "http://one.org/"] pageRef:[4, 19, "http://one.org/"]
        Text[4, 19] chars:[4, 19, "http: … .org/"]
      Text[19, 24] chars:[19, 24, " bar "]
    Code[24, 30] textOpen:[24, 25, "`"] text:[25, 29, "code"] textClose:[29, 30, "`"]
      Text[25, 29] chars:[25, 29, "code"]
    TextBase[30, 50] chars:[30, 50, " baz  … .org/"]
      Text[30, 35] chars:[30, 35, " baz "]
      AutoLink[35, 50] text:[35, 50, "http://two.org/"] pageRef:[35, 50, "http://two.org/"]
        Text[35, 50] chars:[35, 50, "http: … .org/"]
````````````````````````````````


with anchor ref

```````````````````````````````` example Autolink: 3
foo http://one.org/#anchor bar `code` baz http://two.org/#anchor
.
<p>foo <a href="http://one.org/#anchor">http://one.org/#anchor</a> bar <code>code</code> baz <a href="http://two.org/#anchor">http://two.org/#anchor</a></p>
.
Document[0, 65]
  Paragraph[0, 65]
    TextBase[0, 31] chars:[0, 31, "foo h …  bar "]
      Text[0, 4] chars:[0, 4, "foo "]
      AutoLink[4, 26] text:[4, 26, "http://one.org/#anchor"] pageRef:[4, 19, "http://one.org/"] anchorMarker:[19, 20, "#"] anchorRef:[20, 26, "anchor"]
        Text[4, 26] chars:[4, 26, "http: … nchor"]
      Text[26, 31] chars:[26, 31, " bar "]
    Code[31, 37] textOpen:[31, 32, "`"] text:[32, 36, "code"] textClose:[36, 37, "`"]
      Text[32, 36] chars:[32, 36, "code"]
    TextBase[37, 64] chars:[37, 64, " baz  … nchor"]
      Text[37, 42] chars:[37, 42, " baz "]
      AutoLink[42, 64] text:[42, 64, "http://two.org/#anchor"] pageRef:[42, 57, "http://two.org/"] anchorMarker:[57, 58, "#"] anchorRef:[58, 64, "anchor"]
        Text[42, 64] chars:[42, 64, "http: … nchor"]
````````````````````````````````


with intellij dummy identfier

```````````````````````````````` example(Autolink: 4) options(intellij-dummy)
foo http://one.org/#⎮anchor bar `code` baz http://two.org/#anchor
.
<p>foo <a href="http://one.org/#⎮anchor">http://one.org/#⎮anchor</a> bar <code>code</code> baz <a href="http://two.org/#anchor">http://two.org/#anchor</a></p>
.
Document[0, 65]
  Paragraph[0, 65]
    TextBase[0, 32] chars:[0, 32, "foo h …  bar "]
      Text[0, 4] chars:[0, 4, "foo "]
      AutoLink[4, 27] text:[4, 27, "http://one.org/#%1fanchor"] pageRef:[4, 19, "http://one.org/"] anchorMarker:[19, 20, "#"] anchorRef:[20, 27, "%1fanchor"]
        Text[4, 27] chars:[4, 27, "http: … nchor"]
      Text[27, 32] chars:[27, 32, " bar "]
    Code[32, 38] textOpen:[32, 33, "`"] text:[33, 37, "code"] textClose:[37, 38, "`"]
      Text[33, 37] chars:[33, 37, "code"]
    TextBase[38, 65] chars:[38, 65, " baz  … nchor"]
      Text[38, 43] chars:[38, 43, " baz "]
      AutoLink[43, 65] text:[43, 65, "http://two.org/#anchor"] pageRef:[43, 58, "http://two.org/"] anchorMarker:[58, 59, "#"] anchorRef:[59, 65, "anchor"]
        Text[43, 65] chars:[43, 65, "http: … nchor"]
````````````````````````````````


```````````````````````````````` example(Autolink: 5) options(no-autolink)
http://example.com/one. Example 2 (see http://example.com/two). Example 3: http://example.com/foo_(bar)
.
<p>http://example.com/one. Example 2 (see http://example.com/two). Example 3: http://example.com/foo_(bar)</p>
.
Document[0, 103]
  Paragraph[0, 103]
    Text[0, 103] chars:[0, 103, "http: … (bar)"]
````````````````````````````````


```````````````````````````````` example Autolink: 6
http://example.com/one. Example 2 (see http://example.com/two). Example 3: http://example.com/foo_(bar)
.
<p><a href="http://example.com/one">http://example.com/one</a>. Example 2 (see <a href="http://example.com/two">http://example.com/two</a>). Example 3: <a href="http://example.com/foo_(bar)">http://example.com/foo_(bar)</a></p>
.
Document[0, 104]
  Paragraph[0, 104]
    TextBase[0, 103] chars:[0, 103, "http: … (bar)"]
      AutoLink[0, 22] text:[0, 22, "http://example.com/one"] pageRef:[0, 22, "http://example.com/one"]
        Text[0, 22] chars:[0, 22, "http: … m/one"]
      Text[22, 39] chars:[22, 39, ". Exa … (see "]
      AutoLink[39, 61] text:[39, 61, "http://example.com/two"] pageRef:[39, 61, "http://example.com/two"]
        Text[39, 61] chars:[39, 61, "http: … m/two"]
      Text[61, 75] chars:[61, 75, "). Ex … e 3: "]
      AutoLink[75, 103] text:[75, 103, "http://example.com/foo_(bar)"] pageRef:[75, 103, "http://example.com/foo_(bar)"]
        Text[75, 103] chars:[75, 103, "http: … (bar)"]
````````````````````````````````


make sure www. auto links are recognized

```````````````````````````````` example Autolink: 7
www.example.com
.
<p><a href="http://www.example.com">www.example.com</a></p>
.
Document[0, 16]
  Paragraph[0, 16]
    TextBase[0, 15] chars:[0, 15, "www.e … e.com"]
      AutoLink[0, 15] text:[0, 15, "www.example.com"] pageRef:[0, 15, "www.example.com"]
        Text[0, 15] chars:[0, 15, "www.e … e.com"]
````````````````````````````````


```````````````````````````````` example Autolink: 8
foo@example.com
.
<p><a href="mailto:foo@example.com">foo@example.com</a></p>
.
Document[0, 16]
  Paragraph[0, 16]
    TextBase[0, 15] chars:[0, 15, "foo@e … e.com"]
      MailLink[0, 15] text:[0, 15, "foo@example.com"]
        Text[0, 15] chars:[0, 15, "foo@e … e.com"]
````````````````````````````````


```````````````````````````````` example Autolink: 9
foo@com
.
<p>foo@com</p>
````````````````````````````````


```````````````````````````````` example Autolink: 10
<http://example.com>
.
<p><a href="http://example.com">http://example.com</a></p>
.
Document[0, 21]
  Paragraph[0, 21]
    AutoLink[0, 20] open:[0, 1, "<"] text:[1, 19, "http://example.com"] pageRef:[1, 19, "http://example.com"] close:[19, 20, ">"]
````````````````````````````````


Issue #37, How to add attribute 'class' to AutoLink node

```````````````````````````````` example Autolink: 11
https://google.com/abc?hello=456&world=789
.
<p><a href="https://google.com/abc?hello=456&amp;world=789">https://google.com/abc?hello=456&amp;world=789</a></p>
.
Document[0, 43]
  Paragraph[0, 43]
    TextBase[0, 42] chars:[0, 42, "https … d=789"]
      AutoLink[0, 42] text:[0, 42, "https://google.com/abc?hello=456&world=789"] pageRef:[0, 42, "https://google.com/abc?hello=456&world=789"]
        Text[0, 42] chars:[0, 42, "https … d=789"]
````````````````````````````````


Issue #62, Autolinks extension for http:// and https:// links includes trailing spaces

```````````````````````````````` example Autolink: 12
http:// some text

https:// some text

ftp:// some text

file:// some text


.
<p><a href="http://">http://</a> some text</p>
<p><a href="https://">https://</a> some text</p>
<p><a href="ftp://">ftp://</a> some text</p>
<p><a href="file://">file://</a> some text</p>
.
Document[0, 77]
  Paragraph[0, 18] isTrailingBlankLine
    TextBase[0, 17] chars:[0, 17, "http: …  text"]
      AutoLink[0, 7] text:[0, 7, "http://"] pageRef:[0, 7, "http://"]
        Text[0, 7] chars:[0, 7, "http://"]
      Text[7, 17] chars:[7, 17, " some text"]
  Paragraph[19, 38] isTrailingBlankLine
    TextBase[19, 37] chars:[19, 37, "https …  text"]
      AutoLink[19, 27] text:[19, 27, "https://"] pageRef:[19, 27, "https://"]
        Text[19, 27] chars:[19, 27, "https://"]
      Text[27, 37] chars:[27, 37, " some text"]
  Paragraph[39, 56] isTrailingBlankLine
    TextBase[39, 55] chars:[39, 55, "ftp:/ …  text"]
      AutoLink[39, 45] text:[39, 45, "ftp://"] pageRef:[39, 45, "ftp://"]
        Text[39, 45] chars:[39, 45, "ftp://"]
      Text[45, 55] chars:[45, 55, " some text"]
  Paragraph[57, 75] isTrailingBlankLine
    TextBase[57, 74] chars:[57, 74, "file: …  text"]
      AutoLink[57, 64] text:[57, 64, "file://"] pageRef:[57, 64, "file://"]
        Text[57, 64] chars:[57, 64, "file://"]
      Text[64, 74] chars:[64, 74, " some text"]
````````````````````````````````


Don't process fenced code blocks

```````````````````````````````` example Autolink: 13
```
http://example.com 
```
.
<pre><code>http://example.com 
</code></pre>
.
Document[0, 28]
  FencedCodeBlock[0, 27] open:[0, 3, "```"] content:[4, 24] lines[1] close:[24, 27, "```"]
    Text[4, 24] chars:[4, 24, "http: … com \n"]
````````````````````````````````


## Source Position Attribute

```````````````````````````````` example(Source Position Attribute: 1) options(src-pos)
foo@example.com
http://example.com
.
<p md-pos="0-34"><a md-pos="0-15" href="mailto:foo@example.com">foo@example.com</a>
<a md-pos="16-34" href="http://example.com">http://example.com</a></p>
.
Document[0, 34]
  Paragraph[0, 34]
    TextBase[0, 15] chars:[0, 15, "foo@e … e.com"]
      MailLink[0, 15] text:[0, 15, "foo@example.com"]
        Text[0, 15] chars:[0, 15, "foo@e … e.com"]
    SoftLineBreak[15, 16]
    TextBase[16, 34] chars:[16, 34, "http: … e.com"]
      AutoLink[16, 34] text:[16, 34, "http://example.com"] pageRef:[16, 34, "http://example.com"]
        Text[16, 34] chars:[16, 34, "http: … e.com"]
````````````````````````````````


## Issue 178

Issue #178, AutolinkExtension does not add http:// to simple urls starting with www.

```````````````````````````````` example Issue 178: 1
auto link www.example.com
.
<p>auto link <a href="http://www.example.com">www.example.com</a></p>
.
Document[0, 26]
  Paragraph[0, 26]
    TextBase[0, 25] chars:[0, 25, "auto  … e.com"]
      Text[0, 10] chars:[0, 10, "auto link "]
      AutoLink[10, 25] text:[10, 25, "www.example.com"] pageRef:[10, 25, "www.example.com"]
        Text[10, 25] chars:[10, 25, "www.e … e.com"]
````````````````````````````````


## Issue 217

Issue #217, Escape an Autolink

```````````````````````````````` example(Issue 217: 1) options(ignore-google)
auto link www.google.com
.
<p>auto link www.google.com</p>
.
Document[0, 24]
  Paragraph[0, 24]
    Text[0, 24] chars:[0, 24, "auto  … e.com"]
````````````````````````````````


## Issue 265

Issue #265, Autolink extension converts autolinks in inline code nodes in code

```````````````````````````````` example Issue 265: 1
`http://..../wiki`
.
<p><code>http://..../wiki</code></p>
.
Document[0, 19]
  Paragraph[0, 19]
    Code[0, 18] textOpen:[0, 1, "`"] text:[1, 17, "http: … //..../wiki"] textClose:[17, 18, "`"]
      Text[1, 17] chars:[1, 17, "http: … /wiki"]
````````````````````````````````


## Issue xxx-1

Issue, Autolink extension does not convert URI prefix without following text

```````````````````````````````` example Issue xxx-1: 1
http://
.
<p><a href="http://">http://</a></p>
.
Document[0, 8]
  Paragraph[0, 8]
    TextBase[0, 7] chars:[0, 7, "http://"]
      AutoLink[0, 7] text:[0, 7, "http://"] pageRef:[0, 7, "http://"]
        Text[0, 7] chars:[0, 7, "http://"]
````````````````````````````````


```````````````````````````````` example Issue xxx-1: 2
http://abc http://
.
<p><a href="http://abc">http://abc</a> <a href="http://">http://</a></p>
.
Document[0, 19]
  Paragraph[0, 19]
    TextBase[0, 18] chars:[0, 18, "http: … tp://"]
      AutoLink[0, 10] text:[0, 10, "http://abc"] pageRef:[0, 10, "http://abc"]
        Text[0, 10] chars:[0, 10, "http://abc"]
      Text[10, 11] chars:[10, 11, " "]
      AutoLink[11, 18] text:[11, 18, "http://"] pageRef:[11, 18, "http://"]
        Text[11, 18] chars:[11, 18, "http://"]
````````````````````````````````


```````````````````````````````` example Issue xxx-1: 3
test http://
.
<p>test <a href="http://">http://</a></p>
.
Document[0, 13]
  Paragraph[0, 13]
    TextBase[0, 12] chars:[0, 12, "test  … tp://"]
      Text[0, 5] chars:[0, 5, "test "]
      AutoLink[5, 12] text:[5, 12, "http://"] pageRef:[5, 12, "http://"]
        Text[5, 12] chars:[5, 12, "http://"]
````````````````````````````````


```````````````````````````````` example Issue xxx-1: 4
test http://   
.
<p>test <a href="http://">http://</a></p>
.
Document[0, 16]
  Paragraph[0, 16]
    TextBase[0, 12] chars:[0, 12, "test  … tp://"]
      Text[0, 5] chars:[0, 5, "test "]
      AutoLink[5, 12] text:[5, 12, "http://"] pageRef:[5, 12, "http://"]
        Text[5, 12] chars:[5, 12, "http://"]
````````````````````````````````


```````````````````````````````` example Issue xxx-1: 5
test custom.protocol-1+2://   
.
<p>test <a href="custom.protocol-1+2://">custom.protocol-1+2://</a></p>
.
Document[0, 31]
  Paragraph[0, 31]
    TextBase[0, 27] chars:[0, 27, "test  … +2://"]
      Text[0, 5] chars:[0, 5, "test "]
      AutoLink[5, 27] text:[5, 27, "custom.protocol-1+2://"] pageRef:[5, 27, "custom.protocol-1+2://"]
        Text[5, 27] chars:[5, 27, "custo … +2://"]
````````````````````````````````


```````````````````````````````` example Issue xxx-1: 6
test custom.protocol-1+2://abc
.
<p>test <a href="custom.protocol-1+2://abc">custom.protocol-1+2://abc</a></p>
.
Document[0, 31]
  Paragraph[0, 31]
    TextBase[0, 30] chars:[0, 30, "test  … //abc"]
      Text[0, 5] chars:[0, 5, "test "]
      AutoLink[5, 30] text:[5, 30, "custom.protocol-1+2://abc"] pageRef:[5, 30, "custom.protocol-1+2://abc"]
        Text[5, 30] chars:[5, 30, "custo … //abc"]
````````````````````````````````


## Issue xxx.2

Issue of autolinks not parsed after built in auto link

```````````````````````````````` example Issue xxx.2: 1
<http://test.com> www.vladsch.com
.
<p><a href="http://test.com">http://test.com</a> <a href="http://www.vladsch.com">www.vladsch.com</a></p>
.
Document[0, 34]
  Paragraph[0, 34]
    AutoLink[0, 17] open:[0, 1, "<"] text:[1, 16, "http://test.com"] pageRef:[1, 16, "http://test.com"] close:[16, 17, ">"]
    TextBase[17, 33] chars:[17, 33, " www. … h.com"]
      Text[17, 18] chars:[17, 18, " "]
      AutoLink[18, 33] text:[18, 33, "www.vladsch.com"] pageRef:[18, 33, "www.vladsch.com"]
        Text[18, 33] chars:[18, 33, "www.v … h.com"]
````````````````````````````````


