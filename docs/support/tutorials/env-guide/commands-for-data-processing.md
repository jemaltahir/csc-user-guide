# Commands for data processing

Linux provides plenty of tools to study, filter and modify data files.
These tools are often useful when data files need to be modified from
one format to another or when specific data needs to be extracted from
large data sets. However, plain linux tools have their limitations. If
you are going to do more complex data management, scripting languages
like Python and Perl may be more efficient tools to use. In the case of
numerical data many application programs, for example R,
provides tools for both complex analysis tasks and automating the
analysis.

## Grep selects rows that match a given string

The ***grep*** command is used to select rows from a file. This
command is very useful for picking data from large files, however using
*grep* requires that you are well aware of the contents of the file you
are working with. The basic syntax of *grep* is:

    grep pattern target_file

This command goes through the target file and prints out rows that
contain the given search pattern.  
For example, say we have a file called *authors.txt* that contains
following rows:

    Eeva Pekkanen. Oulu 50
    Markus Aho. Turku 50
    Pekka Niemi. Tampere 26
    Leena Virtanen. Kuopio 32

In that case, the command :

    grep Pekka authors.txt

Would print two rows:

    Eeva Pekkanen. Oulu 50
    Pekka Niemi. Tampere 26

as also *Pekkanen* matches the search string *Pekka*. If we want to use
full names as search strings, including the space character between the
names, we need to embrace the search string within quotation marks.

    grep "Pekka Niemi" authors.txt

prints out just:

    Pekka Niemi. Tampere 26

An inverse selection is done using option ***-v*** . For example, the
command:

    grep -v Pekka authors.txt

prints out :

    Markus Aho. Turku 50
    Leena Virtanen. Kuopio 32

By default *grep* is case sensitive and thus the command:

    grep pekka authors.txt

would not result any hits. With the option *-i* , *grep* ignores the
difference between upper and lower case letters.

    grep -i pekka authors.txt

prints again:

    Eeva Pekkanen. Oulu 50
    Pekka Niemi. Tampere 2

Grep interprets the given search string as a *regular expression*, where
some characters have a special meaning and they are interpreted as part
of the regular expression string. The same regular expression syntax is
used with ***awk*** and ***sed*** commands too. For example:

-   the dot (**.**) is used to define any single character.
-   **^** means the beginning of a line
-   **$** means the end of a line
-   **\[ \]** matches any of the characters inside the brackets. For
    example \[abc\] would match a,b or c.
-   **\[^ \]** matches any character, except the characters inside the
    brackets. For example \[^abc\] would select all rows that contain
    also other characters than just a,b and c.
-   **\*** matchs zero or more of the preceding character or expression
-   **\\{n,m\\}** matches *n* to *m* occurrences of the preceding
    character or expression

For example:

    grep "i." authors.txt

prints the following two rows as the search criterion is in this case:
*i and any other character*.

    Pekka Niemi. Tampere 26
    Leena Virtanen. Kuopio 32

In these kind of situations you can use the *backslash character* (\\)
to define that the following character should not be interpreted as part
of a regular expression. Thus the command:

    grep "i\." authors.txt

now returns just one row:

    Pekka Niemi. Tampere 26

*Grep* is often used to filter the output of other commands. For
example, to so see what files in the current directory are from the year
2010 we can use command *ls -l* and pipe it to *grep*. With *grep* we
can select from the output only those rows that include the string 2010.
However, some file names may contain the string "2010", even though they
are not dating from 2010. To make the filtering more effective, we can
use quotation marks to include the space before and after the year
number to the search string. `ls -l | grep " 2010 "`. In addition to the
normal *grep* command, there exist several other grep-like tools. For
example *zgrep* or *bzgrep* can be used to search rows from files
compressed with *gzip* or *bzip2*.


## Using sed to select rows by row number

The stream editor ***sed*** can be used to select and modify rows
from a text file. Many of the *grep* operations, described above can be
done with *sed* too. The *sed* command is discussed a bit more in detail
in the chapter *2.5.8 Replacing characters and strings*. Here we only
show how *sed* can be used to select a certain row from a file. For
example the third row of the file *authors.txt* could be selected with
command `sed -n "3"p authors.txt` In this case the result is:
`Pekka Niemi. Tampere 26` This row picking method is handy in loops. For
example the following loop would print out three rows from the example
file:

```bash
for i in 4 3 2
do
  row=$(sed -n "$i"p authors.txt)
  echo "Row $i looks like: $row"
done
```


## Simple column selection with cut

The ***cut*** command provides a simple tool to select
character *regions* (**-c** ) or *columns* (**-f**) from a linux text
file. When selecting by character numbers the syntax of the command is:

    cut -cstart-end

For example command:

    cut -c2-5 authors.txt

prints text from the second to the fifth character from each row of the
*authors.txt* file. Assuming this is the same file that was used in the
grep examples above, the output looks like:

    eva
    arku
    ekka
    eena

With option **-f** columns can be printed. By default *tabulator* is
used and the column delimiter, other delimiters can be defined with the
option **-d** . For example, to select the first and third column
(**-f1,3**) from the file authors.txt, we need to define *space* as the
delimiter (**-d " "**):

    kkayttaj@c305:~> cut -d " " -f1,3 authors.txt
    Eeva Oulu
    Markus Turku
    Pekka Tampere
    Leena Kuopio


## Using awk to work with columns

***Awk*** is a scripting language that can be used to process text
files in linux environments. *Awk* is a rather old programming language,
and nowadays more modern scripting tools are normally used for creating
data management scripts. However, due to very simple syntax *awk* is
still frequently used to perform simple filtering and editing tasks. In
this guide we don't provide a general overview to *awk* programming.
Instead we show some examples how *awk* can be used to work with files
or as a part of linux command pipeline. In the simple command line
usage, the basic syntax of *awk* is: awk -F "*field delimiter*"
'*statement* {*command1*}{*command2*}...' *infile.dat* *Awk* can be used
to process the input data as columns. By default *awk* uses both space
and tab characters as column delimiters (note that unlike the *cut*
command, *awk* interprets all successive spaces as one single column
separator). The columns can be accessed in awk with variables named *$1*
(the first column), *$2* (second column), *$3* (third column) etc. *$0*
can be used to refer to "all the columns". For example, the following
command prints the first and third column of the file authors.txt.

    kkayttaj@c305:~> awk '{print $1 $3}' authors.txt
    EevaOulu
    MarkusTurku
    PekkaTampere
    LeenaKuopio

In the example above, the fields are not separated in the output. To do
that you add the space or tabulator character (***\\t***) to the print
statement. You can also add other text to the output. Then next example
adds text and changes the order of columns.

    kkayttaj@c305:~> awk '{print "Location: "$3"\t Name: "$1" "$2}' authors.txt
    Location: Oulu   Name: Eeva Pekkanen.
    Location: Turku  Name: Markus Aho.
    Location: Tampere   Name: Pekka Niemi.
    Location: Kuopio    Name: Leena Virtanen.

A more sophisticated way to do the modification above is to use the
*printf* command instead of *print*. The syntax of the *awk* *printf*
command is similar to that of the linux command *printf* discussed in
chapter 2.7.5.

    awk '{printf "Location:%s\tName: %s %s \n", $3, $1, $2}' authors.txt

One of the nice features of *awk* is that you can use conditional
statements in the command. You can compare both strings and numbers. For
example, the following command prints data only when the third column is
"Tampere"

    kkayttaj@c305:~> awk '{if ( $3 == "Tampere")  print "Location: "$3"\tName: "$1" "$2}' authors.txt
    Location: Tampere       Name: Pekka Niemi.

Alternatively, the condition could be defined in the statement part
before the *awk* commands:

    awk ' $3 == "Tampere" {print "Location: "$3"\t Name: "$1" "$2}' authors.txt

If the columns contain numerical values, *awk* can also do numerical
operations. For example, let's use *awk* to calculate how much disk
space jpg image files take in the current folder. We can first list the
jpg files with command: **ls -l \*.jpg**

    kkayttaj@c305:~> ls -l *.jpg
    -rw-------+ 1 kkayttaj csc   85112 Dec 19  2002 Image_2.jpg
    -rw-r--r--+ 1 kkayttaj csc  105542 Nov  6  2006 Screen.jpg
    -rw-r--r--+ 1 kkayttaj csc  167598 May 15  2008 casa1.jpg
    -rw-------+ 1 kkayttaj csc  550031 Mar 25  2008 hexa.jpg
    -rw-------+ 1 kkayttaj csc  869534 Dec 23  2005 img.jpg
    -rw-------+ 1 kkayttaj csc   19159 Jul 23  2003 kuva.jpg
    -rw-r-xr-x+ 1 kkayttaj csc  433268 Dec 23  2005 plot3.jpg
    -rw-------+ 1 kkayttaj csc 1591444 Jan 26 12:27 copy.jpg

To sum up the values of the file sizes in the fifth column, we pipe the
output to awk:

    kkayttaj@c305:~>ls -l *.jpg | awk '{total+=$5} END {print "JPG files in total: "total}'
    JPG files in total: 3821688

Note that in the *awk* command above the *total* is a variable name used
by *awk*. The *END* defines that the following command is executed only
when all input lines are processed. In the second *awk* computing
example, we calculate and display a cumulative sum and average of ages
from the file authors.txt.

    awk '{ tot_age = tot_age + $4 }{ aver=tot_age/NR}{ print $0 " cumulative age sum:" tot_age " average:" aver}' authors.txt

Here we use the automatic *NR* row count variable to calculate average
of the age. This command prints out:

    Eeva Pekkanen. Oulu 50 cumulative age sum:50 average:50
    Markus Aho. Turku 50 cumulative age sum:100 average:50
    Pekka Niemi. Tampere 26 cumulative age sum:126 average:42
    Leena Virtanen. Kuopio 32 cumulative age sum:158 average:39.5

 

## Using sort to order rows

The ***sort*** command can be used to order rows of a text file or
other input data into alphabetical or numerical order. The syntax of the
*sort* command is simple: `sort -options files_to_sort` By default the
*sort* command orders the rows of the file alphabetically using case
sensitive sorting. With option **-f**, case insensitive sorting is
performed. If the data contains columns, separated by the space or
tabulator character you can ask the sort command to use a certain column
(or columns) for ordering the data. The columns to be used can be
selected with the option **-k** *column number*. For example, to sort
the data in the file *authors.txt* (the example file used previously in
this chapter), according to the family names that are located in the
second column, you use command:

    sort -k 2 authors.txt

That prints out:

    Markus Aho. Turku 50
    Pekka Niemi. Tampere 26
    Eeva Pekkanen. Oulu 50
    Leena Virtanen. Kuopio 32

You can also define other column separators with option **-t**
*separator*. Note that by default, *sort* orders also numbers
alphabetically. Option **-n** makes *sort* to use a numerical ordering
instead. The option **-r** reverses the order. Numerical sorting
according to the fourth column can now be done with the command

    sort -k4 -n authors.txt

Result:

    Pekka Niemi. Tampere 26
    Leena Virtanen. Kuopio 32
    Eeva Pekkanen. Oulu 50
    Markus Aho. Turku 50

You can also define that several columns will be used for sorting. For
example, the following command uses the numerical order of the fourth
column as the primary sorting criteria ( **-k4n** ). In cases where
several rows have the same value in the fourth column, the alphabetic
order is used as the secondary sorting criteria (**-k2**)

    sort -k4n -k2 authors.txt

In this case, the result is:

    Pekka Niemi. Tampere 26
    Leena Virtanen. Kuopio 32
    Markus Aho. Turku 50
    Eeva Pekkanen. Oulu 50

The sorted data can be saved to a new file either by using option **-o**
or redirecting the standard output to a file with **&gt;**. For example,
both commands below create the same file containing the sorted version
of the file *authors.txt*.

    sort -k4n -k2 -o authors_sorted.txt authors.txt
    sort -k4n -k2 authors.txt > authors_sorted.txt

## Removing duplicate rows with uniq

The command ***uniq*** is often used in combination with the
*sort* command to remove redundancy from a data set. The *uniq* command
checks if two successive rows in the input file or standard input are
identical. By default, *uniq* removes the successive duplicate rows.
Note that *uniq* does not check if an identical row exists elsewhere in
the input. Because of that, the data is normally sorted before applying
the *uniq* command. As an example, say we have a file called *names*
that contains following rows:

    Pekka
    Pekka
    Leena
    Markus
    Eeva
    Leena

Running command:

    uniq names

will give following output:

    Pekka
    Leena
    Markus
    Eeva
    Leena

In the output, row "*Leena*" still occurs twice as the two identical
names were not in the successive rows. The situation can be fixed by
sorting the rows before *uniq* is applied, for example:

    sort names | uniq

prints the following output

    Eeva
    Leena
    Markus
    Pekka

It is often useful to know, how many identical rows were found. This
information can be added to the output of *uniq* with the option *-c*.
For example:

    sort names | uniq -c

Gives the following output:

          1 Eeva
          2 Leena
          1 Markus
          2 Pekka

Note that also *space* and *tabulator* characters are used when *uniq*
compares the rows. Thus a row containing "Leena" is not identical to a
row containing "Leena ". By default *uniq* is case sensitive, but using
option **-i** makes *uniq* to ignore the cases and consider "leena" and
"Leena" to be identical. However, note that if you use case insensitive
*uniq*, you may need to do also the sorting in case insensitive mode
using command *sort -f*.

## Replacing characters with tr

The ***tr*** command ("translate") is used to replace individual
characters. *Tr* reads the standard input and writes the translated data
to the standard output. The syntax of *tr* command is

    tr search_character replacement_character

For example, dots (.) in the file *authors.txt* could be replaced with
commas (,) with command:

    tr "." "," < authors.txt

this prints out:

    Eeva Pekkanen, Oulu 50
    Markus Aho, Turku 50
    Pekka Niemi, Tampere 26
    Leena Virtanen, Kuopio 32

Note that the command above does not modify the original input file in
any way. In the examples here, the results of *tr* command are printed
to the screen. When *tr* is used to modify large files, the output
should of course be redirected to a new file instead of the screen. For
example:

    tr "." "," < authors.txt > authors_mod.txt

If you would like to remove the dots, you could use the option **-d**
with just one character set, telling what characters are to be removed:

    tr -d "."  < authors.txt

Note that the *tr* translates individual characters, not words. Thus
command *tr "Oulu" "Turku"* would not change the word "Oulu" to "Turku".
Instead it would do the following character conversions to all the text
O-&gt;T, l-&gt;r, u-&gt;k. The *tr* command can do translations with
special characters like tabulator (\\t) and newline (\\n) and with
predefined character sets like all lower case letters (\[:lower:\]) and
all upper case letters (\[:upper:\] ). For example:

    tr "." "\n" < authors.txt

would replace the dots with line breaks in the file *authors.txt* :

    Eeva Pekkanen
     Oulu 50
    Markus Aho
     Turku 50
    Pekka Niemi
     Tampere 26
    Leena Virtanen
     Kuopio 32

Modifying the file so that all the text is written with upper case
letters can be done with:

     tr "[:lower:]" "[:upper:]" < authors.txt

Resulting:

    EEVA PEKKANEN, OULU 50
    MARKUS AHO, TURKU 50
    PEKKA NIEMI, TAMPERE 26
    LEENA VIRTANEN, KUOPIO 32

Check the manual page of *tr* (*man tr*) to see the list of all special
characters and character sets that *tr* can use.

 

## Replacing words and strings with sed

Replacing text strings can be done with ***sed***. *Sed* is a stream
editor that can be used for many different text processing operations.
*Sed* reads string data, either from a file or piped from another
command, and does the edition operations defined by the user and then
prints the edited string to the standard output. *Sed* is a very
powerful tool for automatic text editing, though a bit hard to learn. In
this guide we do not provide a general introduction to *sed,* as even a
modest overview would take several pages. Instead we show few practical
examples how *sed* can be used. *Sed* is commonly used with the syntax:

    sed -e sed_script input-file

The *sed script* is typically short formulation that defines what kind
of editing should be done. For example to do a replacement operation we
could use the *sed* operation *s/"search string"/"replacement
string"/g*. In this formulation *s* means search and replace operation.
The *g* in the end of the formulation means that the replacement
operation is global i.e. all matching strings will be replaced. You
could replace, for example, just the first or second occurrence of the
search string on a row by using number 1 or 2 instead of *g*. For
example the operation where Oulu is replaced by Turku in the file
*authors.txt* can be done with the command:

    sed -e s/"Oulu"/"Turku"/g authors.txt

Result:

    Eeva Pekkanen. Turku 50
    Markus Aho. Turku 50
    Pekka Niemi. Tampere 26
    Leena Virtanen. Kuopio 32

Just like the *grep* command, *sed* interprets the given search string
as a *regular expression*. This means that some characters are
interpreted as special regular expression defining characters. For
example the dot (.) is used to define any single character. Thus the
*sed* command *s/"."/","/g* would change not just dots but all
characters to commas (,). In these kind of situations you can use the
*backslash* character (\\) to tell the *sed* command that the following
character should not be interpreted as part of a regular expression. On
the other hand using the regular expressions can make *sed* very
effective. Below are some regular expression examples applied to the
*authors.txt* file with *sed*.

### Sed Example 1

Using the $ character to define the end of a line (note
the single quotation marks (' ) that prevent the $ character to be
interpreted as bash variable indicator) :

    sed -e s/'0$'/"1 changed"/g authors.txt

Prints out:

    Eeva Pekkanen. Oulu 51 changed
    Markus Aho. Turku 51 changed
    Pekka Niemi. Tampere 26
    Leena Virtanen. Kuopio 32

 

### Sed Example 2

Using dot (.) to define any single character:

    sed -e s/"e.a"/"EXA"/g authors.txt

Prints out:

    EEXA Pekkanen. Oulu 50
    Markus Aho. Turku 50
    Pekka Niemi. Tampere 26
    LeEXA Virtanen. Kuopio 32

### Sed Example 3

Using "^" to define beginning of a line.

    sed -e s/"^P"/"START:P"/g authors.txt

Prints out:

    Eeva Pekkanen. Oulu 50
    Markus Aho. Turku 50
    START:Pekka Niemi. Tampere 26
    Leena Virtanen. Kuopio 32

