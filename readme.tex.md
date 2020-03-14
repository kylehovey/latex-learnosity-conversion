# LaTeX to Learnosity - Conversion Documentation

---

## Overview

$\LaTeX$ is a markdown language used mostly in the academic field to typeset documents. In order to use it, you must have a TeX distribution installed on your computer. This distribution consists of a program that will convert `.tex` files into a `PDF` or other graphical format. When composing a document, writers will often include packages that extend the base functionality of the language. Distributions are often large due to the (frankly bad) common practice of including every package you could ever use in the distribution download. This guide covers a minimal TeX install for OS X (and potentially Linux, just use your own package manager instead of `brew`).

---

## Prerequisites

### Package Manager

The method I am outlining here is the most reliable one that I have found, and it involves using a package manager called `brew`. You will need this both to install your $\LaTeX$ distribution, as well as a convenient utility called `pandoc` that will do the conversion to the format that Learnosity expects in their editor source. Follow the instructions [on their site](https://brew.sh/) to install `brew` on your computer.

### $\LaTeX$ distribution

Next you will need to install a $\LaTeX$ distribution on your machine. I recommend installing `basictex`, as it is a much smaller download than many other distributions (which can come out to `4GB` for a download, and `>7GB` when installed). Open up your terminal on your computer and run:

```bash
brew cask install basictex
```

### Pandoc

Pandoc is the bread and butter of this process. `pandoc` is a program that lets you convert between almost any pair of document formats. In this case, we are using it to convert $\LaTeX$ to `HTML` with Mathjax embedded equations. Learnosity's questions use Mathjax (a $\LaTeX$-like language) within their `HTML` to render equations. If you were to open up the source for a question body with math in it, for example, you might see:

```html
<p>What is the integral of <span class="math display">\(x^x\)</span>?</p>
```

The Learnosity equation editor is just producing the $\LaTeX$ code inside the `\( . . . \)`, which denotes an embedded math expression. Pandoc will convert any $\LaTeX$ `.tex` document you are given into `html` with the Mathjax embedded equations inside.

To install `pandoc`, in a terminal run:

```bash
brew install pandoc
```

---

## Conversion

The rest is fairly easy. In terminal, navigate to the directory where you have your `.tex` documents using these commands:

* `ls` - Lists contents of directory you are inside of
* `cd <dir>` - Steps into directory `<dir>`

Once inside the directory with your `.tex` file(s), run this command (substitute `file.tex` with the file you want to convert, and `file.html` with the file you want to create):

```bash
pandoc file.tex -o file.html -t html5 --mathjax
```

This should take less than a second to run and produce `HTML` output in `file.html` (or whatever file you specified in the command). Open that file in a text/code editor and then copy/paste from it into the Source text area in the Learnosity question editor when you need to convert the body of a question. All formatting should be the same as it would have been in the test originally.

---

## Example

This repository contains an example `.tex` file. If you want to test this technique, download this repository and in a terminal run:

```bash
# First cd into the repository before running the below
pandoc test.tex -o file.html -t html5 --mathjax
```

You can check the output of `file.html` against the following to see if it worked:

```html
<h1 id="some-really-cool-math" class="unnumbered">Some Really Cool Math</h1>
<p>One way to think about the integral is as a continuous discrete sum (<span class="math inline">\(\int \rightarrow \sum\)</span>). Much in the same way the derivative is phrased as the limiting case of a finite difference over decreasing interval <span class="math inline">\(\lim_{h \rightarrow 0} \frac{f(x+h)-f(x)}{h}\)</span>, an integral is defined in the context of sums over intervals of decreasing width subtending an area incident to the function and its independent variable’s axis.</p>
<p><span class="math display">\[\displaystyle \int_0^\infty e^{-x} dx = 1\]</span></p>
```
