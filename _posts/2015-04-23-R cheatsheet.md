---
layout: post
title: "R cheatsheet"
description: "r"
category: notes
tags: [notes]
---
<https://cran.r-project.org/doc/contrib/Short-refcard.pdf>

*by Tom Short, EPRI PEAC, tshort@epri-peac.com 2004-11-07
Granted to the public domain. See www.Rpad.org for the source and latest version. Includes material from R for Beginners by Emmanuel Paradis (with
permission).*

###Getting help
Most R functions have online documentation.  
**help(topic)** documentation on topic  
**?topic id.**  
**help.search("topic")** search the help system  
**apropos("topic")** the names of all objects in the search list matching  
the regular expression ”topic”  
**help.start()** start the HTML version of help  
**str(a)** display the internal *str*ucture of an R object  
**summary(a)** gives a “summary” of a, usually a statistical summary but it is
generic meaning it has different operations for different classes of a  
**ls()** show objects in the search path; specify pat="pat" to search on a
pattern  
**ls.str()** str() for each variable in the search path
**dir()** show files in the current directory  
**methods(a)** shows S3 methods of a
methods(class=class(a)) lists all the methods to handle objects of
class a  
###Input and output
**load()** load the datasets written with save  
**data(x)** loads specified data sets  
**library(x)** load add-on packages  
**read.table(file)** reads a file in table format and creates a data
frame from it; the default separator sep="" is any whitespace; use
header=TRUE to read the first line as a header of column names; use
as.is=TRUE to prevent character vectors from being converted to factors;
use comment.char="" to prevent "#" from being interpreted as
a comment; use skip=n to skip n lines before reading data; see the
help for options on row naming, NA treatment, and others  
**read.csv("filename",header=TRUE)** id. but with defaults set for
reading comma-delimited files  
**read.delim("filename",header=TRUE)** id. but with defaults set
for reading tab-delimited files  
**read.fwf(file,widths,header=FALSE,sep="",as.is=FALSE)** 
read a table of f ixed width f ormatted data into a ’data.frame’; widths
is an integer vector, giving the widths of the fixed-width fields  
**save(file,...)** saves the specified objects (...) in the XDR platformindependent
binary format  
**save.image(file)** saves all objects  
**cat(..., file="", sep=" ")** prints the arguments after coercing to
character; sep is the character separator between arguments  
**print(a, ...)** prints its arguments; generic, meaning it can have different
methods for different objects  
**format(x,...)** format an R object for pretty printing  
**write.table(x,file="",row.names=TRUE,col.names=TRUE,
sep=" ")** prints x after converting to a data frame; if quote is TRUE,
character or factor columns are surrounded by quotes ("); sep is the
field separator; eol is the end-of-line separator; na is the string for
missing values; use col.names=NA to add a blank column header to
get the column headers aligned correctly for spreadsheet input  
**sink(file)** output to file, until sink()
Most of the I/O functions have a file argument. This can often be a character
string naming a file or a connection. file="" means the standard input or
output. Connections can include files, pipes, zipped files, and R variables.
On windows, the file connection can also be used with description =
"clipboard". To read a table copied from Excel, use
x <- read.delim("clipboard")
To write a table to the clipboard for Excel, use
write.table(x,"clipboard",sep="\t",col.names=NA)
For database interaction, see packages RODBC, DBI, RMySQL, RPgSQL, and
ROracle. See packages XML, hdf5, netCDF for reading other file formats.
###Data creation
**c(...)** generic function to combine arguments with the default forming a
vector; with recursive=TRUE descends through lists combining all
elements into one vector  
**from:to** generates a sequence; “:” has operator priority; 1:4 + 1 is “2,3,4,5”  
**seq(from,to)** generates a sequence by= specifies increment; length=
specifies desired length  
**seq(along=x)** generates 1, 2, ..., length(along); useful for for
loops  
**rep(x,times)** replicate x times; use each= to repeat “each” element
of x each times; rep(c(1,2,3),2) is 1 2 3 1 2 3;
rep(c(1,2,3),each=2) is 1 1 2 2 3 3
**data.frame(...)** create a data frame of the named or unnamed
arguments; data.frame(v=1:4,ch=c("a","B","c","d"),n=10);
shorter vectors are recycled to the length of the longest  
**list(...)** create a list of the named or unnamed arguments;
list(a=c(1,2),b="hi",c=3i);  
**array(x,dim=)** array with data x; specify dimensions like
dim=c(3,4,2); elements of x recycle if x is not long enough  
**matrix(x,nrow=,ncol=)** matrix; elements of x recycle  
**factor(x,levels=)** encodes a vector x as a factor
