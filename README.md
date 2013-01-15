
Kindlize
========

Tired and/or guilty of printing out arXiv pre-prints everyday? Frustrated by
the experience of reading two-column journal articles on your e-reader?

If you happen to own a [Kindle
DX](http://www.amazon.com/Kindle-DX-Wireless-Reader-3G-Global/dp/B002GYWHSQ)
device, and happen to be an avid [arXiv](http://arxiv.org/) reader, Kindlize
will make reading arXiv pre-prints on e-ink screen as pleasant as on A4 papers!

The idea is simple. arXiv usually keeps the LaTeX source file of uploaded
manuscripts, and Kindlize will reformat and recompile those LaTeX files so
that the new pdf file is Kindle-friendly.


Screenshots
-----------

### Before (pure text and equations)

![Screenshot](http://bitbucket.org/nye17/kindlize/raw/default/screenshots/textpage_before_small_framed.png)


### After (pure text and equations)

![Screenshot](http://bitbucket.org/nye17/kindlize/raw/default/screenshots/textpage_after_small_framed.png)


### Before (with Figure)

![Screenshot](http://bitbucket.org/nye17/kindlize/raw/default/screenshots/figpage_before_small_framed.png)

### After (with Figure)

![Screenshot](http://bitbucket.org/nye17/kindlize/raw/default/screenshots/figpage_after_small_framed.png)


Requirements
------------

* python2.7
* a LaTeX environment
* rsync
* [Dropbox](http://db.tt/i5xwlaj9) (suggested)


Installation
------------

    hg clone ssh://hg@bitbucket.org/nye17/kindlize 
    cd kindlize
    python setup.py install

or if you want to install to a specified directory `INSTALLDIR`

    python setup.py install --prefix=INSTALLDIR


Workflow
--------

It is useful to explain the most common workflow using Kindlize. There are three major goals we want to achieve to establish a hassle-free reading experience:

* Converting the tarball downloaded from arXiv into Kindle-friendly pdf.
* Maintaining a local directory of "kindlized" pdfs among multiple computers.
* Synchronizing the Kindle arXiv content to the local directory. 

Kindlize is designed to take care of all three goals with some help from
Dropbox. To kindlize an arXiv article, simply do

    kindlize 1211.1379 astrocoffee

where [`1211.1379`](http://arxiv.org/abs/1211.1379) is the arXiv identifier for
the paper you want to read, and `astrocoffee` is the name of the directory
you want to keep this file under your filesystem. In particular, I have
created a local directory  `~/Dropbox/kindle_sync/` for storing kindlized
pdfs, and this pdf (turns out to be, accidentally, `Zu12.pdf`) will go into
`~/Dropbox/kindle_sync/astrocoffee` directory. I will talk more about the
filesystem in the next section.

The above command will also enable you to preview the kindlized pdf using
your favorite pdfviewer, and save a copy to your local arXiv directory.

I have created a new directory under Kindle `documents/incoming/` to
synchronize to the `~/Dropbox/kindle_sync/` directory on my computer. To
synchronize, simply plug in your Kindle and make sure that the Kindle
filesystem is accessible, then run

    kindlize

without arguments. This will force the Kindle directory to be exactly the
same as your local directory (i.e., files could be either added or deleted
from your Kindle!). This command will also send the newly added pdfs to the
right `collection` on your Kindle. For example, `Zu12.pdf` file will show
up in the `incoming - astrocoffee` collection, however, you have to reboot
your Kindle to see it in action.



Configuration
-------------

Obviously, Kindlize needs to understand the filesystems of both your computer
and Kindlize device - where to put temporary files during compiling, where
to put kindlized pdfs on your computer and on your Kindle, etc. For this
purpose, Kindlize needs to read a configuration file under your home directory
`~/.kindle.cfg`. You can modify the `example.cfg` file that comes with the
source package and copy it to your local home directory.

The required items are

## [general]

### device=Kindle DX

Currently Kindlize only serves the Kindle DX device, as other Kindle versions
are too small for serious journal reading. But if you want to give it a shot,
please let me know (I don't personally own other Kindles, so I may need your
help on tests).

## [directory]

### tmpDir=~/tmp

Determines where the TeX file will be unpacked and compiled into pdfs.

### dropDir=~/Dropbox/kindle_sync/

Determines where on your local directory you want to store a backup of your
Kindlized pdfs.

### incomingDir=/media/Kindle/documents/Incoming/

Tells Kindlize where your Kindle content will show up in your file system
and which subdirectory should you store the arXiv files. `incomingDir`
will always be synced to `dropDir`.

## [LaTeX]

### font=charter

Determines which LaTeX font package to use for pdfs. You have to
make sure they are available in your LaTeX environment if you do
`\usepackage{YOUR_CHOICE_OF_FONT}`.

### fontheight=14pt

Determines the height of your fonts.

### fontwidth=18pt

Determines the baseline of your fonts.

## [pdf]

### pdfviewer

Determines which pdf viewer to preview the generated pdfs.

Contributing
------------

Please, report bugs and issues as I won't be able to cover all the TeX formats on arXiv. Feel free to send me ideas through the [issue tracker][] or pull requests!

[issue tracker]: http://github.com/nye17/kindlize/issues
