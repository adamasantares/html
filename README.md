html\Tag
========

Just an experiment of HTML tags constructor

How to use
----------

The firs thing is it's prefer to use ["] char for $properties.

```php
/**
* @param string|array Elements properties
* @param string|Tag|Tag[] String line, Tag element or array of Tag elements
*/
tg($properties, $children);
```

```php
tg("tagName .class .class .class #id ^nameAttr @rel !typeAttribute %contentAttribute", 'content of tag or attribute');
```

```php
tg("tagName .class ^nameAttr 'Simple text content {$variable}'"); // this is why it's better to use ["] instead [']
```

```php
tg(['tagName.class.class.class#id^nameAttr@rel!typeAttribute%contentAttribute', 'attr' => 'value'], 'content of tag or attribute');
```

Simple examples
---------------

```php
tg('p', 'Hello');  -->  <p>Hello</p>
tg("p 'Hello'");  -->  <p>Hello</p>

tg(".content 'Hello'");  -->  <div class="content">Hello</div>  // by default a tag's name is "DIV"

tg("meta^viewport", 'width=device-width, initial-scale=1');  -->  <meta name="viewport" content="width=device-width, initial-scale=1">

tg("script@text/javascript%src '/js/script.js'");  -->  <script type="text/javascript" src="/js/script.js"></script>
```

Detailed examples
-----------------

###Example 1

```tg("script #jquery !text/javascript %src '/jquery.min.js'");``` or ```tg("script#jquery!text/javascript%src", '/jquery.min.js');``` 

There are:

 - "script" - is a tag's name
 - "#jquery" - is value for "id" attribute
 - "!text/javascript" - is value for "type" attribute
 - "%src" - is name of attribute for content
     
Result: ```<script id="jquery" type="text/javascript" src="/jquery.min.js"></script>``` 
    
###Example 2

```tg("link @icon !image/x-icon '/favicon.ico'");```

There are:

 - "link" - is a tag's name
 - "@icon" - is value for "rel" attribute
 - "!image/x-icon" - is value for "type" attribute
 
A single tag ("void element" or "empty element") has no body, so TG can automatically set content to special content attribute.
So, for LINK it is "href" attribute, for IMG it is "src", for META it is "content" attribute
and for INPUT it is "value" attribute. It means that you don't need to define "content attribute" by "%" token.
In current example we didn't use "%href" in properties argument, but "href" will get "/favicon.ico" by auto. 
  
Result: ```<link rel="icon" type="image/x-icon" href="/favicon.ico">```
        
###Example 3

```tg(['a.button', 'href' => '/url/path'], tg(['img.my-img', 'alt' => 'just-a-link'], '/image.png'));```

There are 2 elements: link "A" and image "IMG". For both elements we used array for $properties, there its structure:

```["properties string", "attribute" => "value", "attribute" => "value"]```

The first value of array (with "0" key) must be an properties string, tag's attributes must follow by it.

Result: ```<a class="button" href="/url/path"><img class="my-img" src="/image.png" alt="just-a-link"></a>```

###Example 4
 
```tg("input!text^option '123'");```

For Input tag no need to define name of attribute for value (as says in Example 2).

There are:

 - "input" - is a tag's name
 - "!text" - is value for "type" attribute
 - "^option" - is value for "name" attribute

Result: ```<input type="text" name="option" value="123">```

###Example 5

```tg(".super.puper.element#for_content 'My content'");```

There are:
 
 - no tag's name (by default it is "DIV")
 - ".super.puper.element" - are classes names
 - "#for_content" - is value for "id" attribute

Result: ```<div id="for_content" class="super puper element">My content</div>```

###Example 6

This example shows how to put inner tags. Also here you can see 'br/' string as inner element.
It will converted to <br> tag (for 'hr/' the same behaviour).

```php
tg('form.signin', [
    tg('label', ['User name', 'br/',
        tg("input!text^username '{$username}'")
    ]),
    tg('label', ['Password', 'br/',
        tg("input!password^password", '')
    ]),
    tg(['input!password^password:readonly', 'title' => 'tooltip text'], 'Only for read'),
    'br/',
    tg("button 'Login'")
]);
```

### Example 7

This example of how to create &lt;select&gt; tag.

```php
tg('select', ['option1', 'option2', 'option3']); // for <option>'s "value" attribute will used key of array element

tg('select', ['value1' => 'title1', 'value2' => ['title2', 'disabled'], 'value3' => ['title3', 'selected', 'data-something' => 'somevalue]]);
```

More examples
-------------

See index.php file for more examples.

```
$ php -S localhost:3003
```
