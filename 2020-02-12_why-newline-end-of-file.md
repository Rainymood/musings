# Why do files need to end with a newline? 

From [here](https://evanhahn.com/newline-necessary-at-the-end-of-javascript-files/)

This actually comes from the [C standard](http://c0x.coding-guidelines.com/5.1.1.2.html) 5.1.1.12 line 123

>123 A source file that is not empty shall end in a new-line character, which shall not be immediately preceded by a backslash character before any such splicing takes place.

It also prevents things like this when concatenating two files

```
return 12; }var x = 10;
```

and 

```
x = 0; // x is now 0function doStuff() {
```

and this

```
x = 0function doStuff() {
```

When in doubt, newline out! 