---
layout:     post
title:      "Code Highlight Sample"
subtitle:   "For Code block higlight test"
date:       2016-11-16
header-img: "img/post-bg-default.jpg"
author:     darcylee
catalog:    true
tags:       [highlight, sample]

---

# 说明

目前使用rouge作为highlighter

#### 查看支持的themes

```shell
$ rougify help style
usage: rougify style [<theme-name>] [<options>]

Print CSS styles for the given theme.  Extra options are
passed to the theme.  Theme defaults to thankful_eyes.

options:
  --scope	(default: .highlight) a css selector to scope by

available themes:
  base16, base16.dark, base16.monokai, base16.monokai.light, base16.solarized, base16.solarized.dark, colorful, github, molokai, monokai, monokai.sublime, thankful_eyes

```

#### 支持的语言

| 语言                  | aliases                         | 说明                                                                                                                       |
| :----:                | :----:                          | :----                                                                                                                      |
| apache                |                                 | configuration files for Apache web server                                                                                  |
| applescript           | applescript                     | The AppleScript scripting language by Apple Inc. (http://developer.apple.com/applescript/)                                 |
| c                     |                                 | The C programming language                                                                                                 |
| clojure               | clj,cljs                        | The Clojure programming language (clojure.org)                                                                             |
| coffeescript          | coffee,coffee-script            | The Coffeescript programming language (coffeescript.org)                                                                   |
| common_lisp           | cl,common-lisp,elisp,emacs-lisp | The Common Lisp variant of Lisp (common-lisp.net)                                                                          |
| conf                  | config,configuration            | A generic lexer for configuration files                                                                                    |
| cpp                   | c++                             | The C++ programming language                                                                                               |
| csharp                | c#,cs                           | a multi-paradigm language targeting .NET                                                                                   |
| css                   |                                 | Cascading Style Sheets, used to style web pages                                                                            |
| dart                  |                                 | The Dart programming language (dartlang.com)                                                                               |
| diff                  | patch,udiff                     | Lexes unified diffs or patches                                                                                             |
| elixir                | elixir,exs                      | Elixir language (elixir-lang.org)                                                                                          |
| erb                   | eruby,rhtml                     | Embedded ruby template files                                                                                               |
| erlang                | erl                             | The Erlang programming language (erlang.org)                                                                               |
| factor                |                                 | Factor, the practical stack language (factorcode.org)                                                                      |
| gherkin               | cucumber,behat                  | A business-readable spec DSL ( github.com/cucumber/cucumber/wiki/Gherkin )                                                 |
| glsl                  |                                 | The GLSL shader language                                                                                                   |
| go                    | go,golang                       | The Go programming language (http://golang.org)                                                                            |
| groovy                |                                 | The Groovy programming language (groovy.codehaus.org)                                                                      |
| haml                  | HAML                            | The Haml templating system for Ruby (haml.info)                                                                            |
| handlebars            | hbs,mustache                    | the Handlebars and Mustache templating languages                                                                           |
| haskell               | hs                              | The Haskell programming language (haskell.org)                                                                             |
| html                  |                                 | HTML, the markup language of the web                                                                                       |
| http                  |                                 | http requests and responses                                                                                                |
| ini                   |                                 | the INI configuration format                                                                                               |
| io                    |                                 | The IO programming language (http://iolanguage.com)                                                                        |
| java                  |                                 | The Java programming language (java.com)                                                                                   |
| javascript            | js                              | JavaScript, the browser scripting language                                                                                 |
| json                  |                                 | JavaScript Object Notation (json.org)                                                                                      |
| json-doc              |                                 | JavaScript Object Notation with extenstions for documentation                                                              |
| liquid                |                                 | Liquid is a templating engine for Ruby (liquidmarkup.org)                                                                  |
| literate_coffeescript | litcoffee                       | Literate coffeescript                                                                                                      |
| literate_haskell      | lithaskell,lhaskell,lhs         | Literate haskell                                                                                                           |
| llvm                  |                                 | The LLVM Compiler Infrastructure (http://llvm.org/)                                                                        |
| lua                   |                                 | Lua (http://www.lua.org)                                                                                                   |
| make                  | makefile,mf,gnumake,bsdmake     | Makefile syntax                                                                                                            |
| markdown              | md,mkd                          | Markdown, a light-weight markup language for authors                                                                       |
| matlab                | m                               | Matlab                                                                                                                     |
| moonscript            | moon                            | Moonscript (http://www.moonscript.org)                                                                                     |
| nginx                 |                                 | configuration files for the nginx web server (nginx.org)                                                                   |
| nim                   | nimrod                          | The Nim programming language (http://nim-lang.org/)                                                                        |
| objective_c           | objc                            | an extension of C commonly used to write Apple software                                                                    |
| ocaml                 |                                 | Objective CAML (ocaml.org)                                                                                                 |
| perl                  | pl                              | The Perl scripting language (perl.org)                                                                                     |
| php                   | php,php3,php4,php5              | The PHP scripting language (php.net)                                                                                       |
| plaintext             | text                            | A boring lexer that doesn't highlight anything [aliases: text]                                                             |
| powershell            | posh                            | powershell [aliases: posh]                                                                                                 |
| praat                 |                                 | The Praat scripting language (praat.org)                                                                                   |
| prolog                | prolog                          | The Prolog programming language (http://en.wikipedia.org/wiki/Prolog)                                                      |
| properties            |                                 | .properties config files for Java                                                                                          |
| puppet                | pp                              | The Puppet configuration management language (puppetlabs.org)                                                              |
| python                | py                              | The Python programming language (python.org)                                                                               |
| qml                   | qml                             | QML, a UI markup language                                                                                                  |
| r                     | r,R,s,S                         | The R statistics language (r-project.org)                                                                                  |
| racket                |                                 | Racket is a Lisp descended from Scheme (racket-lang.org)                                                                   |
| ruby                  | rb                              | The Ruby programming language (ruby-lang.org)                                                                              |
| rust                  | rs                              | The Rust programming language (rust-lang.org)                                                                              |
| sass                  |                                 | The Sass stylesheet language language (sass-lang.com)                                                                      |
| scala                 | scala                           | The Scala programming language (scala-lang.org)                                                                            |
| scheme                |                                 | The Scheme variant of Lisp                                                                                                 |
| scss                  |                                 | SCSS stylesheets (sass-lang.com)                                                                                           |
| sed                   |                                 | sed, the ultimate stream editor                                                                                            |
| shell                 | bash,zsh,ksh,sh                 | Various shell languages, including sh and bash                                                                             |
| slim                  |                                 | The Slim template language                                                                                                 |
| smalltalk             | st,squeak]                      | The Smalltalk programming language                                                                                         |
| sml                   | ml                              | Standard ML                                                                                                                |
| sql                   |                                 | Structured Query Language, for relational databases                                                                        |
| swift                 |                                 | Multi paradigm, compiled programming language developed by Apple for iOS and OS X development. (developer.apple.com/swift) |
| tcl                   |                                 | The Tool Command Language (tcl.tk)                                                                                         |
| tex                   | TeX,LaTeX,latex                 | The TeX typesetting system                                                                                                 |
| toml                  |                                 | the TOML configuration format (https://github.com/mojombo/toml)                                                            |
| tulip                 | tlp                             | The tulip programming language http://github.com/jneen/tulip                                                               |
| vb                    | visualbasic                     | Visual Basic                                                                                                               |
| viml                  | vim,vimscript,ex                | VimL, the scripting language for the Vim editor (vim.org)                                                                  |
| xml                   |                                 | <desc for="this-lexer">XML</desc>                                                                                          |
| yaml                  | yml                             | Yaml Ain't Markup Language (yaml.org)                                                                                      |



# Sample

#### C

```c
void main (char *argc char **argv)
{
    return ;
}
```

#### ruby

```ruby
module Jekyll

  class PrismBlock < Liquid::Block
    include Liquid::StandardFilters

    OPTIONS_SYNTAX = %r{^([a-zA-Z0-9.+#-]+)((\s+\w+(=[0-9,-]+)?)*)$}

    def initialize(tag_name, markup, tokens)
      super
      if markup.strip =~ OPTIONS_SYNTAX
        @lang = $1
        if defined?($2) && $2 != ''
          tmp_options = {}
          $2.split.each do |opt|
            key, value = opt.split('=')
            if value.nil?
              value = true
            end
            tmp_options[key] = value
          end
          @options = tmp_options
        else
          @options = { "linenos" => "" }
        end
      else
        raise SyntaxError.new("Syntax Error in 'prism' - Valid syntax: prism <lang> [linenos(='1-5')]")
      end
    end

    def render(context)
      code = h(super).strip

      if @options["linenos"] == true
        @options["linenos"] = "1-#{code.lines.count}"
      end

      <<-HTML
<div>
  <pre data-line='#{@options["linenos"]}'><code class='language-#{@lang}'>#{code}</code></pre>
</div>
      HTML
    end
  end

end

Liquid::Template.register_tag('prism', Jekyll::PrismBlock)

```

#### makefile

```makefile
BUILDDIR      = _build
EXTRAS       ?= $(BUILDDIR)/extras

.PHONY: main clean

main:
	@echo "Building main facility..."
	build_main $(BUILDDIR)

clean:
	rm -rf $(BUILDDIR)/*
```

#### markdown

```markdown
# hello world

you can write text [with links](http://example.com) inline or [link references][1].

* one _thing_ has *em*phasis
* two __things__ are **bold**

[1]: http://example.com

---

hello world
===========

<this_is inline="xml"></this_is>

> markdown is so cool

    so are code segments

1. one thing (yeah!)
2. two thing `i can write code`, and `more` wipee!
```

#### elisp

```elisp
(defun prompt-for-cd ()
   "Prompts
    for CD"
   (prompt-read "Title" 1.53 1 2/4 1.7 1.7e0 2.9E-4 +42 -7 #b001 #b001/100 #o777 #O777 #xabc55 #c(0 -5.6))
   (prompt-read "Artist" &rest)
   (or (parse-integer (prompt-read "Rating") :junk-allowed t) 0)
  (if x (format t "yes") (format t "no" nil) ;and here comment
  )
  ;; second line comment
  '(+ 1 2)
  (defvar *lines*)                ; list of all lines
  (position-if-not #'sys::whitespacep line :start beg))
  (quote (privet 1 2 3))
  '(hello world)
  (* 5 7)
  (1 2 34 5)
  (:use "aaaa")
  (let ((x 10) (y 20))
    (print (+ x y))
  )
```

#### bash

```bash
#!/bin/bash

###### CONFIG
ACCEPTED_HOSTS="/root/.hag_accepted.conf"
BE_VERBOSE=false

if [ "$UID" -ne 0 ]
then
 echo "Superuser rights required"
 exit 2
fi

genApacheConf(){
 echo -e "# Host ${HOME_DIR}$1/$2 :"
}
```

#### java

```java
/**
 * @author John Smith <john.smith@example.com>
*/
package l2f.gameserver.model;

public abstract class L2Char extends L2Object {
  public static final Short ERROR = 0x0001;

  public void moveTo(int x, int y, int z) {
    _ai = null;
    log("Should not be called");
    if (1 > 5) { // wtf!?
      return;
    }
  }
}
```

#### json

```json
[
  {
    "title": "apples",
    "count": [12000, 20000],
    "description": {"text": "...", "sensitive": false}
  },
  {
    "title": "oranges",
    "count": [17500, null],
    "description": {"text": "...", "sensitive": false}
  }
]
```

#### objc

```objc
#import <UIKit/UIKit.h>
#import "Dependency.h"

@protocol WorldDataSource
@optional
- (NSString*)worldName;
@required
- (BOOL)allowsToLive;
@end

@property (nonatomic, readonly) NSString *title;
- (IBAction) show;
@end
```

#### shell

```shell
sudo apt-get install gcc
```

#### swift

```swift
let defaultName = "xiaoli"
var userDefinedName: String?;   //默认值为 nil

var NameToUse = userDefinedName ?? defaultName
// userDefinedName 的值为空，所以 NameToUse 的值为 "xiaoli"

userDefinedName = "zhangsan";
var NameToUse1 = userDefinedName ?? defaultName
// userDefinedName 有值，则 NameToUse 的值为 ”zhangsan”
```

#### css

```css
@font-face {
  font-family: Chunkfive; src: url('Chunkfive.otf');
}

body, .usertext {
  color: #F0F0F0; background: #600;
  font-family: Chunkfive, sans;
}

@import url(print.css);
@media print {
  a[href^=http]::after {
    content: attr(href)
  }
}
```

#### less

```less
@import "fruits";

@rhythm: 1.5em;

@media screen and (min-resolution: 2dppx) {
    body {font-size: 125%}
}

section > .foo + #bar:hover [href*="less"] {
    margin:     @rhythm 0 0 @rhythm;
    padding:    calc(5% + 20px);
    background: #f00ba7 url(http://placehold.alpha-centauri/42.png) no-repeat;
    background-image: linear-gradient(-135deg, wheat, fuchsia) !important ;
    background-blend-mode: multiply;
}

@font-face {
    font-family: /* ? */ 'Omega';
    src: url('../fonts/omega-webfont.woff?v=2.0.2');
}

.icon-baz::before {
    display:     inline-block;
    font-family: "Omega", Alpha, sans-serif;
    content:     "\f085";
    color:       rgba(98, 76 /* or 54 */, 231, .75);
}
```

#### tex

```tex
\documentclass{article}
\usepackage[koi8-r]{inputenc}
\hoffset=0pt
\voffset=.3em
\tolerance=400
\newcommand{\eTiX}{\TeX}
\begin{document}
\section*{Highlight.js}
\begin{table}[c|c]
$\frac 12\, + \, \frac 1{x^3}\text{Hello \! world}$ & \textbf{Goodbye\~ world} \\\eTiX $ \pi=400 $
\end{table}
Ch\'erie, \c{c}a ne me pla\^\i t pas! % comment \b
G\"otterd\"ammerung~45\%=34.
$$
    \int\limits_{0}^{\pi}\frac{4}{x-7}=3
$$
\end{document}
```

#### asm

```Assembly
.text

.global connect
connect:
    mov     r3, #2              ; s->sin_family = AF_INET
    strh    r3, [sp]
    ldr     r3, =server_port    ; s->sin_port = server_port
    ldr     r3, [r3]
    strh    r3, [sp, #2]
    ldr     r3, =server_addr    ; s->sin_addr = server_addr
    ldr     r3, [r3]
    str     r3, [sp, #4]
    mov     r3, #0              ; bzero(&s->sin_zero)
    str     r3, [sp, #8]
    str     r3, [sp, #12]
    mov     r1, sp      ; const struct sockaddr *addr = sp

    ldr     r7, =connect_call
    ldr     r7, [r7]
    swi     #0

    add     sp, sp, #16
    pop     {r0}        ; pop sockfd

    pop     {r7}
    pop     {fp, ip, lr}
    mov     sp, ip
    bx      lr

.data
socket_call:   .long 281
connect_call:  .long 283

/* all addresses are network byte-order (big-endian) */
server_addr:            .long 0x0100007f ; localhost
server_port:            .hword 0x0b1a
```

#### javascript

```javascript
/*
 * Execute a callback function & remove it from the Cordova object
 */
Cordova.callback = function() {
    var scId = arguments[0];
    var callbackRef = null;

    var parameters = [];
    for( var i = 1; i < arguments.length; i++ ) {
        //debug.log( "Adding parameter " + arguments[i] );
        parameters[i-1] = arguments[i];
    }
    // Keep reference to callback
    callbackRef = Cordova.callbacks[scId];

    // Even IDs are success-, odd are error-callbacks - make sure we remove both
    if( (scId % 2) !== 0 ) {
        scId = scId - 1;
    }
    // Remove both the success as well as the error callback from the stack
    Cordova.callbacks.splice( scId, 2 );

    // Finally run the callback
    if( typeof callbackRef == "function" ) callbackRef.apply( this, parameters );
};

/*
 * Enable a plugin for use within Cordova
 */
Cordova.enablePlugin = function( pluginName ) {
    // Enable the plugin
    console.log("typeof pluginName");
    Cordova.plugins[pluginName] = true;

    // Run constructor for plugin if available
    if( typeof Cordova.constructors[pluginName] == "function" ) Cordova.constructors[pluginName]();
}
```
