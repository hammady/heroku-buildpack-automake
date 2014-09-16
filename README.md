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

## Caching
It is possible with MultiMake to cache build artifacts and reuse them across builds. By default MultiMake will build each subdirectory as described in MMakefile. To instruct it to store build outputs (and later reuse them) set the environment variable `MULTIMAKE_CACHE_FILES` to list paths of files to cache in semicolon separated list. For example:

<pre>
heroku config:set MULTIMAKE_CACHE_FILES='svm_light/svm_classify;nginx-1.5.13/objs/nginx'
</pre>

will save `svm_light/svm_classify` and `nginx-1.5.13/objs/nginx` after building in a cache directory that is maintained by Heroku across builds. The next build (provided you still have the environment variable) will look for these 2 files in the cache and if found will skip the whole build process in all subdirectories and copies them directly to the build directory. To disable cache at any time, just remove the environment variable and builds will proceed from scratch.

If you want to purge/override the cache you have 2 options:

- Append a fake path to the environment variable so that MultiMake won't find all paths cached, and will trigger the build process then copies these paths to the cache directory, effectively replacing the old cache.
- Install [Heroku Repo plugin](https://github.com/heroku/heroku-repo) and use it to purge the whole cache directory.
