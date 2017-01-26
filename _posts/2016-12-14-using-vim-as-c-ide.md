---
layout: post
title: "Using vim as C IDE"
description: "How to set up vim as a IDE for C programming"
category: 
tags: [vim,linu
---
{% include JB/setup %}

To use the VIM text editor suxessfuly as a C IDE there is a buch of plugins
and programs that could be god to have. Throu out this article i asume you use a computer with Linux on but perhaps it works with Windows to.

# A list of my installed plugins in vim.
{% highlight vim %}
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" " alternatively, pass a path where Vundle should install plugins
" "call vundle#begi('~/some/path/here')

"======== Vundele pagagess ===============
Plugin 'gmarik/Vundle.vim'                                      " let Vundle manage Vundle, required
Plugin 'tomasr/molokai.git'                                     " Collor sheme.
Plugin 'Lokaltog/powerline'                                     " linje i botten av vim
Plugin 'vim-scripts/taglist.vim'
Plugin 'Valloric/YouCompleteMe'
Plugin 'kien/ctrlp.vim'
Plugin 'rdnetto/YCM-Generator'
Plugin 'scrooloose/syntastic'
"Plugin 'vim-scripts/octave.vim--'
Plugin 'sudar/vim-arduino-syntax'
Plugin 'lervag/vimtex'
Plugin 'SirVer/ultisnips'                                       " Sniplet plugin for vim
Plugin 'honza/vim-snippets'                                     " Sniplets for ultisnips

"========= End packages ==================
" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
{% endhighlight %}

The perhaps most important plug in in this list is Vallorics YouCompleteMe that
allows vim to do tab completion. The draw back of that amazing plugin is that it
can be hard to install. It requires both Clang and a version of vim that
support python.
To get autocompletion in the entire project you need to generate a
.ycm\_extra\_conf file.
But first you need to have a Makefile in the root of your project google how to
write them or simply copy my file.

{% highlight Makefile %}
## My generic makefile written by Magnus Sörensen. GPL3.0 Lisense.

## CROSS\_TOLL is used if you need co add a cross platform compiler.
CROSS\_TOLL=
## CC is the compiler you use in this project.
CC=$(CROSS\_TOLL) gcc
TTY=/dev/pts/2
## Target is the source files with out .c or .cpp  tex main.c ~> main
TARGET=    # < Add files to this line linke
OBJECTS=$(TARGET:=.o)
## EXECFILE is the name on the exe file you want
EXECFILE=prog
## LFLAGS is the libary linker flags like -lncurses or -lpthread.
LFLAGS=-pthread -lrt
#LFLAGS=-lncurses -lm
## CFLAGS tells the copmiler to compile with diffrent flaggs mostly -g -Wall
CFLAGS=-g -Wall
## If files is from windows then run
## recode ISO-8859-1..UTF-8 main.c
## or even better for file in *.{c,h} ; do recode ISO-8859-1..UTF-8 $file; done

## Also remember to create ctags in your src dir
##  $ ctags -R

$(TARGET):
	$(CC) $(CFLAGS) $(LFLAGS) -c $@.c

all: clean $(TARGET)
	$(CC) $(CFLAGS) $(LFLAGS) $(OBJECTS) -o $(EXECFILE)

run: clean all
	./$(EXECFILE)

debug: clean all
	cgdb ./$(EXECFILE)

ddd: clean all
	ddd ./$(EXECFILE) &

.PHONY: clean valgrind

valgrind: clean all
	valgrind ./$(EXECFILE)

clean:
	rm -f *.o $(EXECFILE) *.gch
{% endhighlight %}

# The YCM-generator.
Install the YCM generator from your reepo or AUR.
Then edit the template file.

{% highlight shell %}
sudo vim /usr/share/YCM-Generator/template.py
{% endhighlight %}
In there you can find a array that need to contain language specific parameters.
{% highlight python%}
flags = [
        '-Wall',
        '-Wextra',
        '-Werror',
        '-Wc++98-compat',
        '-Wno-long-long',
        '-Wno-variadic-macros',
        '-fexceptions',
        '-DNDEBUG',
        # You 100% do NOT need -DUSE_CLANG_COMPLETER in your flags; only the
        # YCM
        # source code needs it.
        '-DUSE_CLANG_COMPLETER',
        # THIS IS IMPORTANT! Without a "-std=<something>" flag, clang won't
        # know which
        # language to use when compiling headers. So it will guess. Badly. So
        # C++ headers will be compiled as C headers. You don't want that so ALWAYS
        # specify a "-std=<something>".
        # For a C project, you would set this to something like 'c99' instead
        # of 'c++11'.
        #'-std=c++0x',  # Uncomment for C++
        '-std=gnu99',   # Uncomment for gnu C  (Works best on linux)
        #'-std=c99',     # Uncomment for Ansi C 99
        # ...and the same thing goes for the magic -x option which specifies
        # the language that the files to be compiled are written in. This is mostly
        # relevant for c++ headers.
        # For a C project, you would set this to 'c' instead of 'c++'.
        #'-x', 'c++',
        '-x', 'c',
        #Here are the libraries for the RaspberryPi2 or 3:
        #THIS IS THE MONEY MAKER:
        '-isystem', '/usr/bin/../lib64/gcc/x86_64-pc-linux-gnu/6.3.1/../../../../include/c++/6.3.1',
        '-isystem', '/usr/bin/../lib64/gcc/x86_64-pc-linux-gnu/6.3.1/../../../../include/c++/6.3.1/x86_64-pc-linux-gnu',
        '-isystem', '/usr/bin/../lib64/gcc/x86_64-pc-linux-gnu/6.3.1/../../../../include/c++/6.3.1/backward',
        '-isystem', '/usr/include/clang/',
        '-isystem', '/usr/bin/../lib/clang/3.9.1/include',
        '-isystem', '/usr/local/include',
        '-isystem', '/usr/include',
        #'-isystem', '/usr/bin/../lib/gcc/arm-linux-gnueabihf/4.9/include',
        #'-isystem', '/usr/include/arm-linux-gnueabihf',
        '-I', '.'
]

{% endhighlight %}
To generate that go to root of your project and write

{% highlight shell %}
ycm\_generator -v -b make -c gcc -x c .
{% endhighlight %}

# Go to defenition with ctags
A other verry handy tool to have is Ctags. Ctags let you in vim go to
defenition go to the root dir of the directory and write:
{% highlight shell %}
ctags -R
{%  endhighliht %}
