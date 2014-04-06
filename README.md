# Heroku Buildpack: Automake

Build using this buildpack if you have a hierarchy of Makefile-based programs. It will execute automake and autoconf if
Makefile.am, configure.ac are found, respectively. Finally it will execute make then make install.
To make this work create a Makefile.am in the top level directory listing all subdirectories you want to make in order.
For a full reference on how to use subdirectories, check the [GNU automake manual](http://www.gnu.org/software/automake/manual/html_node/Subdirectories.html).


