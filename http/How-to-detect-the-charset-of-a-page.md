It is the precondition for a web page crawler program to parse the fetched content that it have to detect the charset of the page(Especially when it has some non-ASCII characters like Chinese and Japanses).

But how on earth to do this detection? Let's take a look at the corresponding specification from [W3C](http://www.w3.org/TR/html4/charset.html):

1. An HTTP "charset" parameter in a "Content-Type" field
2. A META declaration with "http-equiv" set to "Content-Type" and a value set for "charset"
3. The charset attribute set on an element that designates an external resource

The above describes the priority of the ways to detect the charset of a page: *Http "Content-Type" Field* > *META declaration* > *Charset Attribute(such as the link tag and script tag)* > *ISO-8859-1(which is the default charset)*

As it is in the W3C specification, which it to say, the standard browsers do this job in this way, too.