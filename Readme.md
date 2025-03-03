# GObject Tutorial for beginners

The github page of this tutorial is also available. Click [here](https://toshiocp.github.io/Gobject-tutorial/).

The GitHub page of this tutorial is available.
Click [here](https://toshiocp.github.io/Gobject-tutorial/) to see it.

#### About this tutorial

This tutorial is aimed at beginners who are learning the GObject system.
One of the biggest difficulties in learning the GObject system is understanding its fundamental object oriented strategy.
All the necessary topics are described in [GObject API Reference](https://docs.gtk.org/gobject/).
But it is probably difficult especially for beginners.

The contents of this tutorial are not beyond the documentation.
It just gives you some example codes and diagrams to help you.
Readers should refer to the GObject documentation when learning this tutorial.

#### GObject reference manual has been changed

I have to point out that the GObject documentation above is the new version.
The GNOME documentation website is revised and the GObject reference manual is also changed in August 2021.
The old version of the reference manual is [here](https://developer-old.gnome.org/gobject/stable/).

#### Generating GFM, HTML and PDF

The table of contents are below and you can see all the tutorials by following the link.
However, you can make GFM, HTML or PDF by the following steps.
GFM is 'GitHub Flavored Markdown', which is used in the document files in the GitHub repository.

1. You need Linux operating system, ruby, rake, pandoc and LaTeX system.
2. download the [GObject-tutorial repository](https://github.com/ToshioCP/Gobject-tutorial) and uncompress the files.
3. change your current directory to the top directory of the source files.
4. type `rake` to produce GFM files. The files are generated under `gfm` directory.
5. type `rake html` to produce HTML files. The files are generated under `docs` directory.
6. type `rake pdf` to produce a PDF file. The file is generated under `latex` directory.

This system is the same as the one in the `GTK 4 tutorial` repository.
There's a document `Readme_for_developers.md` in `gfm` directory in the repository.
It describes the details.

#### Contribution

If you have any questions, feel free to post an issue.
If you find any mistakes in the tutorial, post an issue or pull-request.
When you give a pull-request, correct the source files, which are under the 'src' directory, and run `rake` and `rake html`.
Then GFM and HTML files are automatically updated.

## Table of contents

1. [Prerequisites and License](gfm/sec1.md)
1. [GObject](gfm/sec2.md)
1. [Type system and registration process](gfm/sec3.md)
1. [Signals](gfm/sec4.md)
1. [Properties](gfm/sec5.md)
1. [Derivable type and abstract type](gfm/sec6.md)
1. [Derivable and non-abstract type](gfm/sec7.md)
1. [Child class extends parent's function](gfm/sec8.md)
1. [Interface](gfm/sec9.md)
