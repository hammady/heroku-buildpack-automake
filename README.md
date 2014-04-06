# Heroku Buildpack: Automake

Build using this buildpack if you have a hierarchy of Makefile-based programs. 
It will execute make then make install for the top-level directory.
To build complex projects that constist of subdirectories having their own Makefiles,
You can list subdirectories in the root Makefile as stated in this [tutorial](https://www.gnu.org/software/make/manual/html_node/Phony-Targets.html).

## Example Makefile to build subdirs:

<pre>
SUBDIRS = foo bar

subdirs:
        for dir in $(SUBDIRS); do \
                $(MAKE) -C $$dir; \
        done
</pre>

