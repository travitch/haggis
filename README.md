# What is it?

Haggis is a static site generator, written in Haskell. It allows you to write
your blog posts in any format that pandoc supports, using your chosen
directory structure and file names as the site's url structure.

# Installation

Haggis is available via hackage, so you can `cabal install haggis`. For
non-haskellers on debian based distros, the full procedure might look
something like:

    sudo apt-get install cabal-install
    cabal update
    cabal install haggis

# Usage

A blog using haggis has the following directory structure:

    templates/
      archives.html
      multiple.html
      root.html
      single.html
      tags.html
    src/
      ...

The idea is that each kind of "page": the archive index, the tag index, a page
with multiple blog posts on it, a page with a single blog post (and
potentially comments) on it, are all generated from these templates. Each
template is expanded with whatever relevant content is in it, and then bound
at `#content` of the root template.

Under `src/` is the content of your web page. Files with extensions supported
by pandoc will be read and expanded into the single page template (more on
metadata and expansion below). Files with extensions pandoc doesn't understand
will be copied over directly. Haggis will also generate a nice index file for
every directory. One special index file is the index file generated at the
root. This file contains the most recent 10 blog posts. A blog post is defined
as a file with a date in its metadata.

An example markdown blog post might look like this:

    ---
    title: Haggis kicks ass
    date: 2013-02-19
    tags: haggis, whoopass
    author: tycho
    ---
    Gaddamn, [haggis](http://github.com/tych0/haggis) kicks ass!

Haggis will parse the stuff in between the `---`s as metadata, and then
process the rest of the file as a markdown document. If your template is

    <div class="page">
      <h2 class="title"></h2>
      <small>Posted by <span class="author"></span> on <span class="date"></span></small>
      <div class="content"></div>
      <span class="tags">Tags: <a href="#" class="tag"></a></span>
    </div>

You'd end up with something like:

    <div class="page">
      <h2 class="title">Haggis kicks ass</h2>
      <small>Posted by <span class="author">tycho</span> on <span class="date">2013-02-19</span></small>
      <div class="content">
        Gaddamn, <a href="http://github.com/tycho/haggis">haggis</a> kicks ass!
      </div>
      <span class="tags">Tags:
        <a href="/tags/haggis.html" class="tag">haggis</a>
        <a href="/tags/whoopass.html" class="tag">whoopass</a>
      </span>
    </div>

...which would then get inserted into the `#content` element of your root
template. Since this post has a `date` entry in its metadata, it will show up
on the index page if it's one of the 10 most recent such posts.

A full example of haggis source is [here](http://github.com/tych0/tycho.ws),
and the result is [here](http://beta.tycho.ws).

# TODO

  * more sausage jokes
  * comments support
  * custom binders for templates
  * organize code in a sane way, also remove some binding duplication
  * allow custom pandoc options?
  * remove assumption about being installed at root of domain
  * make the root index optional?
  * make tags/archives optional?
