
                             Windows port
                             ============

This directory contains the files required to build this software on the
native Windows platform. This is not a place to look for help if you are
using a POSIX emulator, such as Cygwin. Check the Unix instructions for 
that.



CONTENTS
========

1. General
   1.1 Building From the Command-Line
   1.2 Configuring The Source
   1.3 Compiling
   1.4 Installing

2. Compiler Specifics
   2.1 Microsoft Visual C/C++
   2.1 GNU C/C++, Mingw Edition
   2.2 Borland C++ Builder
       2.2.1 Building with iconv support
	   2.2.2 Compatability problems with MSVC (and probably CYGWIN)
	   2.2.3 Other caveats




1. General
==========


1.1 Building From The Command-Line
----------------------------------

This is the easiest, preferred and currently supported method. It can
be that a subdirectory of the directory where this file resides 
contains project files for some IDE. If you want to use that, please
refer to the readme file within that subdirectory.

In order to build from the command-line you need to make sure that
your compiler works from the command line. This is not always the
case, often the required environment variables are missing. If you are
not sure, test if this works first. If it doesn't, you will first have
to configure your compiler suite to run from the command-line - please
refer to your compiler's documentation regarding that.

The first thing you want to do is configure the source. You can have
the configuration script do this automatically for you. The
configuration script is written in JScript, a Microsoft's
implementation of the ECMA scripting language. Almost every Windows
machine can execute this through the Windows Scripting Host. If your
system lacks the ability to execute JScript for some reason, you must
perform the configuration manually and you are on your own with that.

The second step is compiling the source and, optionally, installing it
to the location of your choosing.


1.2 Configuring The Source
--------------------------

The configuration script accepts numerous options. Some of these
affect features which will be available in the compiled software,
others affect the way the software is built and installed. To see a
full list of options supported by the configuration script, run

  cscript configure.js help

from the win32 subdirectory. The configuration script will present you
the options it accepts and give a biref explanation of these. In every
case you will have two sets of options. The first set is specific to
the software you are building and the second one is specific to the
Windows port.

Once you have decided which options suit you, run the script with that
options. Here is an example:

  cscript configure.js compiler=msvc prefix=c:\opt 
    include=c:\opt\include lib=c:\opt\lib debug=yes

The previous example will configure the process to use the Microsoft's
compiler, install the library in c:\opt, use c:\opt\include and 
c:\opt\lib as additional search paths for the compiler and the linker 
and build executables with debug symbols.

Note: Please do not use path names which contain spaces. This will
fail. Allowing this would require me to put almost everything in the
Makefile in quotas and that looks quite ugly with my
syntax-highlighting engine. If you absolutely must use spaces in paths
send me an email and tell me why. If there are enough of you out there
who need this, or if a single one has a very good reason, I will
modify the Makefile to allow spaces in paths.


1.3 Compiling
-------------

After the configuration stage has been completed, you want to build
the software. You will have to use the make tool which comes with
your compiler. If you, for example, configured the source to build
with Microsoft's MSVC compiler, you would use the NMAKE utility. If
you configured it to build with GNU C compiler, mingw edition, you
would use the GNU make. Assuming you use MSVC, type

  nmake /f Makefile.msvc

and if you use MinGW, you would type

  make -f Makefile.mingw

and if you use Borland's compiler, you would type

  bmake -f Makefile.bcb

in the win32 subdirectory. When the building completes, you will find
the executable files in win32\bin.* directory, where * stands for the
name of the compiler you have used.


1.4 Installing
--------------

You can install the software into the directory you specified to the
configure script during the configure stage by typing (with MSVC in
this example)

  nmake /f Makefile.msvc install

That would be it, enjoy.



2. Compiler Specifics
=====================


2.1 Microsoft Visual C/C++
--------------------------

If you use the compiler which comes with Visual Studio .NET, note that
it will link to its own C-runtime named msvcr70.dll or msvcr71.dll. This 
file is not available on any machine which doesn't have Visual Studio 
.NET installed.


2.2 GNU C/C++, Mingw edition
----------------------------

When specifying paths to configure.js, please use slashes instead of 
backslashes for directory separation. Sometimes Mingw needs this. If
this is the case, and you specify backslashes, then the compiler will 
complain about not finding necessary header files.


2.2.1 Building with iconv support

If you configure libxml2 to include iconv support, you will obviously need to
obtain the iconv library and include files. To get them, just follow the links 
at http://www.gnu.org/software/libiconv/ - there are pre-compiled Win32 
versions available, but note that these where built with MSVC. Hence the 
supplied import library is in COFF format rather than OMF format. 

