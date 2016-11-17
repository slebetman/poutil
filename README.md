# poutil

Simple command line utility to process po files

### Installation

This program requires Tcl 7.4 or higher

On Unix or Unix-like OSes just copy or symlink the script to any directory in your $PATH
or add this directory to your $PATH.

On Windows you can add this folder to your system path
(Google "windows add folder to system path").

### Usage

    syntax: poutil <command> <?filename?> <?arguments...?>

where `filename` is the po file you want to process. If the filename
is `-` then it is read from stdin. This is useful if you want to use
this program as a filter.

And `command` is one of the following:

- **help** : Prints this help message.

- **count** : Count words in the po file.

- **untranslated** : Finds all untranslated messages. Optional argument: `-nocomments` = filter out comments.

- **fuzzy** : Finds all fuzzy messages. Optional argument: `-nocomments` = filter out comments.

- **nocomments** : Outputs the po file without comments.

- **split** : Splits po files evenly into two or more files. Optional argument: number of files (defaults to 2)

- **merge** : Merges changes into a PO file. Argument: file to merge.

- **grep** : Finds all messages matching a regular expression.
  - either a single argument: a regexp
  - or: pairs of section_name and regexp where section_name is one of the following:

           -msgid        = msgid matches the regexp
           -msgid-no     = msgid does not match the regexp
           -msgstr       = msgstr matches the regexp
           -msgstr-no    = msgstr does not match the regexp
           -comments     = comments match the regexp
           -comments-no  = comments do not match the regexp

       more than one pair may be given.

Examples:
1. Find messages containing the word "foo":

       poutil grep file.po foo

2. Find messages where msgid contains the word "foo" which are not fuzzy:

       poutil grep file.po -msgid foo -comments-no fuzzy

3. Get all untranslated messages but omit comments:

       poutil untranslated file.po -nocomments

4. Get all untranslated messages where the msgid contains the word "foo":

       poutil grep file.po -msgstr '^$' -msgid foo

   4b. Alternatively you can use 2 instances of poutil for this:

       poutil untranslated file.po | poutil grep - -msgid foo

5. Find all messages from the file "foo.html"

       poutil grep file.po -comments foo.html

