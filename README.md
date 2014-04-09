# Heroku Buildpack: MultiMake

Build using this buildpack if you have several subdirectories containing Makefile-based programs. You start by defining these subdirectories in a top-level `MMakefile` file along
with their configure options. It will execute `./configure` with your defined options then `make`
in each of the subdirectories.

Combined with [Multipack](https://github.com/ddollar/heroku-buildpack-multi) you can bundle Makefile-based programs with your application, which can be Ruby, Java, Python, ...

## `MMakefile` examples:
The simplest MMakefile lists subdirectories one per line.
<pre>
foo
bar
</pre>
This will first change directory to `foo`, run `./configure` (if found) then `make` if the `./configure` succeeds. Then it will do the same in `bar`.

If you want to add custom configure options you put them after the subdirectory name separated by a colon, like this:

<pre>
...
foo: --with-my-option
...
</pre>
The previous file will run `./configure --with-my-option` inside `foo`.

This flexibility enables one to build several sources, some of them depending on others, take this example:
<pre>
pcre
nginx: --with-pcre=../pcre
</pre>
This will first builds `pcre` with default options then will configure nginx with the pcre that was just compiled.
</pre>

