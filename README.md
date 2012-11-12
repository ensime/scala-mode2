# scala-mode2 — A new scala-mode for emacs

This is a new scala major mode for emacs 24. It is a complete rewrite
based on scala language specification 2.9.

The mode intends to provide the basic emacs support, including
- indenting of code, comments and multi-line strings
- motion commands
- highlighting

Currently the indenting of code has been finalized. Highlighting is
under work. No scala specific motion commands have been added, but
standard emacs motions work ofcourse.

## Setting the mode up for use

1. Make sure you have the latest version of **GNU Emacs** installed. 
The mode has been developed on 24.2 and uses features not available
in emacs prior to version 24.

2. Download the files to a local directory, you can use the *git*
command. This will create a directory called scala-mode2.
```
> git clone git://github.com/hvesalai/scala-mode2.git
```

3. Include the following in your '.emacs'  file. If you have been
using the old scala-mode, make sure it is no longer in load-path.
```
(add-to-list 'load-path "/path/to/scala-mode2/")
(require 'scala-mode)
```

4. That's it. Next you can start emacs and take a look at the
customization menu for scala-mode (use **M-x** *customize-mode* when
in scala-mode or use **M-x** *customize-variable* to customize one
variable).

Free emacs tip: if you are using emacs from a text terminal with dark
background and you are having trouble with colors, try setting the
customization variable *frame-background-mode* to *dark* (use **M-x**
*customize-variable*).

## Indenting modes

*Where four developers meet, there are four opinions on how code should
be indented. Luckily scala-mode already supports 2^4 different ways of 
indenting.*

### Run-on lines (scala-indent:default-run-on-strategy)

The indenting engine has three modes for handling run-on lines. The
**reluctant** (default) mode is geared toward a general style of coding
and the **eager** for strictly functional style. A third mode called
**operators** is between the two.

The difference between the modes is how they treat run-on lines. For
example, the *eager* mode will indent *map* in the following code

```
val x = List(1, 2, 3)
  map(x => x + 1)
```

The *operators* and *eager* modes will indent the second row in the
following code, as the first line ends with opchar.

```
val x = 20 + 
  21
```

The *reluctant* mode (default) will not indent the line in either
case. However, all three modes will indent the second line in these
examples as specified by the *Scala Language Specification*, section
1.2.

```
val x = List(0, 1, 2, 3, 4, 5, 6, 7, 8, 9).
  map (x => x + 1) // last token of previous line can not terminate a statement

val y = (List(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
           map (x => x + 1)) // inside 'newlines disabled' region
```

You can use empty lines in *eager* mode to stop it from indenting a
line. For example

```
val x = foo("bar")
           ("zot", "kala") // indented as curry

val y = foo("bar")

("zot", "kala") // a tuple
```

However, in all three modes pressing **tab** key repeatedly on a line
will toggle between the modes.

### Value expressions (scala-indent:indent-value-expression)

When this variable is set to *t* (default), blocks in value
expressions will be indented one extra step to make the *val*, *var*
or *def* stand out. For example:

```
val x = try {
    some()
  } catch {
    case e => other
  } finally {
    clean-up()
  }
```

When the variable is set to *nil*, the same will indent as:

```
val x = try {
  some()
} catch {
  case e => other
} finally {
  clean-up()
}
```

### Parameter lists (scala-indent:align-parameters)

When this variable is set to *t* (default), parameters and run-on
lines in parameter lists will always align under and acording to the
first parameter.

```
val y = List( "Alpha", "Bravo",
              "Charlie" )

val x = equals(List(1,2,3) map (x =>
                 x + 1))
```

When the variable is set to *nil*, the same will be indented as:

```
val y = List( "Alpha", "Bravo",
    "Charlie" )

val x = equals(List(1,2,3) map (x =>
    x + 1))
```

### Expresison forms: if, for, try (scala-indent:align-forms)

When this variable is set to *t* (default), *if*, *for* and *try*
forms are aligned.

```
val x = if (kala)
          foo
        else if (koira)
          bar
        else
          zot

val x = try "1".toInt
        catch { case e => 0}
        finally { println("hello") }

val xs = for (i <- 1 to 10)
         yield i
```

When the variable is set to *nil*, the same will be indented as:

```
val x = if (kala)
    foo
  else if (koira)
    bar
  else
    zot

val x = try "1".toInt
  catch { case e => 0}
  finally { println("hello") }

val xs = for (i <- 1 to 10)
  yield i
```

## Motion

Basic emacs motion will work as expected. The *forward-sexp* and
*backward-sexp* ( **M-C-f**, **M-C-b** ) motion commands will move over
reserved words, literals, ids and lists.

## Code highlighting

Highlighting code is still a work in progress. Feedback on how it
should work is welcomed as issues to this github project.

## Other features
- supports multi-line strings both in highlight and movement
- highlights only properly formatted string and character constants
- fills scaladoc comments properly ( **M-q** )
- indenting a code line removes trailing whitespace

## Future work

- indent scaladoc left margin correctly (currently scaladoc is not
  indented at all)
- fill row comments after code properly (currently produces error)
- indent and fill multi-line strings with margin correctly
- movement commands to move to previous or next definition (val,
  var, def, class, trait, object)
- highlight headings and annotations inside scaladoc specially (use
  underline for headings)
- highlight variables in string interpolation (scala 2.10)

All suggestions and especially pull requests are welcomed in github
https://github.com/hvesalai/scala-mode2

## Credits

Mode development: Heikki Vesalainen

Contributors and valuable feedback:
- Ray Racine
- Eiríkr Åsheim (aka Erik Osheim)
- Seth Tisue
