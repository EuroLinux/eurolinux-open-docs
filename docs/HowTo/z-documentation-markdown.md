# Quick markdown and extensions guide

The first part of this guide is loosely based on [Adam Pritchard markdown-here
cheetsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).
We include it here for contributors as reference and show how different pieces
will be styled. If you are familiar with markdown, and want to skip to this
project specific extensions they start with [Admonition](#admonition):

### Headers
Headers are created with `#`.
```
# H1 This is is reserved for page title/name
## H2 [Contribution guide] is h2
### H3 [Headers] is h3
...
###### H6
```
#### This is fourth header
##### This is fifth header
###### This is sixth header

**Headers are essentials, because table of content is based on them.**

### Emphasis
```
Emphasis, aka italics, with *asterisks* or _underscores_. 

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~
```
Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

### List

!!! warning "Proper markdown required"
    MK-Docs has proper markdown list ordering that is 1 to 1 with markdown standard.

    See: [Python Markdown Issue 3](https://github.com/Python-Markdown/markdown/issues/3)
    
    **TLDR: You have indent sub-list and paragraps with 4 spaces**

(In this example, leading and trailing spaces are shown with with dots: ⋅)
```
1. First ordered list item
2. Another item
⋅⋅⋅⋅* Unordered sub-list. 
1. Actual numbers don't matter, just that it's a number
⋅⋅⋅⋅1. Ordered sub-list
4. And another item.

⋅⋅⋅⋅To create paragraph within list item you need newline and four leading spaces. To have a line⋅⋅
⋅⋅⋅⋅break without a paragraph, you will need to use two trailing spaces.

!!! info Python markdown quirk
    Python-Markdown won't reset list without paragraph. Even if list types are
    not compatybile (ordered vs unordered)

* Unordered list can use asterisks
- Or minuses
+ Or pluses
```

1. First ordered list item
2. Another item
    * Unordered sub-list. 
1. Actual numbers don't matter, just that it's a number
    1. Ordered sub-list
4. And another item.

    To create paragraph within list item you need newline and four leading spaces. To have a line  
    break without a paragraph, you use two trailing spaces.

!!! info "Python markdown quirk"
    Python-Markdown won't reset list without paragraph. Even if list types are
    not compatybile (ordered vs unordered)

* Unordered list can use asterisks
- Or minuses
+ Or pluses

### Code blocks

To add code block use (without leading space)
```
 ```python
 # nice
 for i in range(69, 420):
   print(i)
 ```
```

Example:
```python
# nice
for i in range(69, 420):
  print(i)
```

### Admonition 

!!! info
    This is admontion extension for markdown. It support things like
    (info,todo), (warning,caution,attention), (danger,error) and more.

Code in markdown:

```
!!! info
    This is admontion extension for markdown. It support things like
    (info,todo), (warning,caution,attention), (danger,error) and more.
```
!!! warning "For more information check documentation"
    Check [mkdocs-material docs](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)

Code in markdown:
```
!!! warning "For more information check documentation"
    Check [mkdocs-material docs](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)
```

### Keyboard Keys

Sometimes you might add keys combinations. For example:


To use second TY terminal use following key combination ++ctrl+alt+f2++

To make them visable in nice way you should use following syntax:
```
To use second TY terminal use following key combination ++ctrl+alt+f2++
```
