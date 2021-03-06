# Pandoc Configuration and Support Files

This serves two functions:

- A set of Pandoc configuration files for Mac OS-X
- A set of instructions for installing Pandoc on a Mac-OS-X system.
Everything is based closely on [Kieran Healy's](https://kieranhealy.org/) [Pandoc/Markdown templates](https://github.com/kjhealy/pandoc-templates).  


## Usage

- clone this repository using:
```
cd ~
git clone https://github.com/kentddaniel/dot-pandoc.git
mv dot-pandoc .pandoc
```
- Follow the installation instructions below

## Keiran Healy's Description

A collection of support files for use with Pandoc, and specifically for helping to turn pandoc markdown files into nice HTML, LaTeX, and PDF output. These files go in your `~/.pandoc/` folder and are designed to work with the style and configuration material provided in [latex-custom-kjh](http://kjhealy.github.com/latex-custom-kjh/), [socbibs](http://kjhealy.github.com/socbibs), and the [Emacs Starter Kit for the Social Sciences](http://kjhealy.github.com/emacs-starter-kit/). The only real dependencies are the latex class and style files in [latex-custom-kjh](http://kjhealy.github.com/latex-custom-kjh/), however.

## Installation and usage (KD):

- Install pandoc and three pandoc filters.  Install these using Homebrew.  
    - Pandoc and the first filters two are easy:
      ```
      brew install pandoc
      brew install pandoc-citeproc
      brew install pandoc-crossref
      ```
    - However, homebrew doesn't have the the [pandoc-citeproc-preamble](https://github.com/spwhitton/pandoc-citeproc-preamble) file, so I had to do the following:
        - First, add *~/.local/bin* to the PATH (by modifying .zshrc, in my case)
        - The install stack, and use stack to install pandoc-citeproc-preamble (following the instructions on the GitHub page, updated to the latest version):
          ```
          brew install stack
          stack install --resolver=lts-10.3 pandoc-citeproc-preamble
          ```

- Get [Marked 2](http://marked2app.com/) (the pandoc viewer).
    - Note that won't work if you get it from the App Store, because of restrictions Apple puts on the code.  So you should get it directly from the [Marked 2 website](http://marked2app.com/).
        - If you already got it from the App Store, and you try to make the changes below, it will not work.  A window will pop up telling you to email them to get a license key.
    - Tell Marked to use
    pandoc as its custom processor. Go to *Marked > Preferences >
    Advanced*. In the *Path:* field, type:
    ```
    /usr/local/bin/pandoc
    ``` 
    - In the 'Args' field below it, like this (but all on one line):
    ```
    -r markdown+simple_tables+table_captions+yaml_metadata_block -w html -s -mart --template=/Users/kent/.pandoc/templates/html.template --filter pandoc-crossref --filter pandoc-citeproc --filter pandoc-citeproc-preamble --bibliography=/Users/kent/bibtex/allrefs.bib
    ```
    Then check the box labeled "Automatically enable for new windows"
- ***LaTeX files:*** 
    - These commands moved the files I wanted.
    ```
    DEST=\`kpsexpand '$TEXMFLOCAL'`
    cd
    mkdir kjhealy_pandoc
    cd kjhealy_pandoc
    git clone https://github.com/kjhealy/latex-custom-kjh.git
    cd laxex-custom-kjh
    find . -name "*.sty" -exec cp -ipv {} $DEST/tex/latex/local \;
    texhash
    ```

- ***Minon Pro fonts:***
- Finally, create a project folder
    - copy the Makefile in ~/.pandoc/Makefile to that folder
    - copy pandoc-crossref-settings.yaml to that folder
    - then, the command "> make [docx,pdf,tex]" will generate that file.


## How I build this ~/.pandoc folder:


- First clone all of Kieran Healy's file into a temporary folder:
  ```
  cd
  mkdir kjhealy_pandoc
  cd kjhealy_pandoc
  git clone https://github.com/kjhealy/latex-custom-kjh.git
  git clone https://github.com/kjhealy/pandoc-templates.git
  git clone https://github.com/kjhealy/md-starter.git
  ```



## Notes

What's included?

- Some [Pandoc](http://johnmacfarlane.net/pandoc/) templates for an
  article in PDF (vita LaTeX) or HTML. These go in
  `~/.pandoc/templates`. These can be be pointed to directly with the
  `--template=` switch as appropriate. The `latex.template` and
  `xelatex.template` depend on the style files in
  [latex-custom-kjh](http://kjhealy.github.com/latex-custom-kjh/).
- I preview HTML documents generated by Pandoc using
  [Marked](http://marked2app.com/), a very handy HTML live previewer
  for markdown files. The `css` files in the `marked/` folder are
  meant to be used together with pandoc and
  [Marked](http://markedapp.com/). The shell script in the `marked/`
  folder, `panmarked.sh` is what I previously had Marked use as a
  custom processor to create its HTML. You point to it in Marked >
  Preferences > Behavior. In the current version of the application,
  Marked 2, this is not needed anymore. You still tell Marked to use
  pandoc as its custom processor. Go to *Marked > Preferences >
  Advanced*. Then specify the file Path to Pandoc like this (e.g.):
  `/usr/local/bin/pandoc` 
  and the various switches and arguments to pandoc
  in the 'Args' field below it, like this (but all on one line):
    ```
    -r markdown+simple_tables+table_captions+yaml_metadata_block -w html -s -S --template=/Users/kent/.pandoc/templates/html.template --filter pandoc-crossref --filter pandoc-citeproc --filter pandoc-citeproc-preamble --bibliography=/Users/kent/bibtex/allrefs.bib
    ```
    Then check the box telling Marked to use this by default. Note
    that you may have to specify the path to any pandoc filters you
    use.
    - When I did this, *Marked 2** crashed, and I got a message that I needed to email them to get a crossref-capable build 
    ***To be resolved***
 - The CSS files can be added in Marked > Style > Custom CSS. Marked
  can then use them to format the HTML output.
      - ***(I'm still not getting this to work)***
- In R, knitr's `knit()` function will turn `.Rmd` files into `.md`
  files. The configuration file in the `knitr/` folder is an example
  to help you produce HTML or `.tex` using knitr's `pandoc()` helper
  function.
- The CSL files in the `csl/` folder format the bibliography generated
  by pandoc and citeproc. (For simplicity we avoid dealing with
  biblatex directly at all.) The `chicago-syllabus.csl` file makes a
  tiny change to a standard Chicago Notes CSL file so you can use it
  to output citation information in the body text of a document. This
  makes it useful for lists of references in CVs and course
  syllabuses. The other two files are APSA and AJPS standard files
  from the main
  [CSL styles repository](https://github.com/citation-style-language/styles).
- The Makefile in the `makefile/` folder helps you generate HTML,
  LaTeX, and PDF output from your markdown files in a convenient
  way. It is meant to go in the folder where you are writing your
  paper. It looks for `.md` files in the working directory and
  converts them to nice HTML, PDF, and LaTeX files using the templates
  provided here, the style files in
  [latex-custom-kjh](http://kjhealy.github.com/latex-custom-kjh/), and
  the bibliography files in
  [socbibs](http://kjhealy.github.com/socbibs). You can of course
  change the bibliography and template files as desired.
- The `pandoc` commands produced by the current version of the `Makefile` include switches that invoke two [pandoc filters](http://pandoc.org/scripting.html) that do additional processing on the bibliography and cross-references in the document. You should install [pandoc-crossref](https://github.com/lierdakil/pandoc-crossref) and [pandoc-citeproc-preamble](https://github.com/spwhitton/pandoc-citeproc-preamble) to make these work.
- The [md-article-starter](https://github.com/kjhealy/md-starter) repository is a basic project folder you can clone that gives you a template for an article written in Markdown and a `Makefile` to produce `.html`, `.tex` or `.pdf` output from it. For R users there is an [rmd-article-starter](https://github.com/kjhealy/rmd-starter) as well, which begins with an `.Rmd` file.

## Contact
Kieran Healy, `@kjhealy`
