---
css: ./decker-users-guide.css
lang: en-US
title: Decker User's Guide
toc-title: Contents
---

🚧 Material that is only relevant for advanced users, developers, true Decker
nerds and Mario is marked with a construction sign (🚧). Eltern haften für ihre
Kinder.

# Description

## Pandoc

Decker uses the universal markup converter
[Pandoc](https://pandoc.org/MANUAL.html#pandocs-markdown) to translate slide
content in Markdown format into interactive HTML slide decks. A working
knowledge of the Pandoc dialect of Markdown is very helpful when working with
Decker.

-   [Pandoc User's Guide](https://pandoc.org/MANUAL.html#pandocs-markdown)

This document mainly describes additional features and conventions that Decker
adds to Pandoc's Markdown.

## Reveal.js

## Features

## Using Decker

## Creating a project

## Working on a project

## Publishing

Decker can use a locally installed [Rsync](https://rsync.samba.org) to publish
the entire project to a remote location with the command

``` sh
> decker publish
```

The remote location is specified in the meta data variable
`publish.rsync.destination:` using the URL formats that Rsync understands. For
example, to publish the entire project directly into the document directory of a
remote webserver the `decker.yaml` file would contain:

``` yaml
publish:
  rsync:
    destination: author@public.server.com:/var/www/html/cg-lectures
```

To more precisely control the behaviour of Rsync, a list of options can be
specified in the variable `publish.rsync.options`. For example, to *mirror* (as
opposed to *copy* ) the public directory to the destination the setting would
be:

``` yaml
publish:
  rsync:
    destination: author@public.server.com:/var/www/html/cg-lectures
    options: 
      - --delete
```

# Targets

Decker uses [Shake](https://shakebuild.com) as it's underlying build and
dependency tracking system.

## `> decker version`

## `> decker decks`

## `> decker html`

## `> decker observed`

## `> decker info`

## `> decker check`

## `> decker publish`

## `> decker search-index`

Builds an inverted index over all Markdown source files and stores it in JSON in
`public/index.json`. The index can be used to implement incremental live-search
over all slides inside a Decker project.

This may well be a little time consuming, so it ist best called only right
before `decker publish`.

# Commands

Commands do not engage the dependency checking and will not trigger rebuilds.

## `> decker clean`

## `> decker example`

## `> decker serve`

## `> decker pdf`

Compiles PDF documents for all HTML decks. It starts a headless Chrome browser
and uses it's printing capabilities to do that.

This may well be a little time consuming, so it ist best called only right
before `decker publish`.

# Options

## `-h`, `--help`

List all decker commands, targets and options.

## `-m key=value`, `--meta="key=value"`

Specifies meta data variables (see below).

## `-S`, `--server`

Serve the public dir via HTTP (implies --watch).

## `-w`, `--watch`

Watch changes to source files and rebuild current target if necessary.

# Meta Data

Meta data variables are specified in YAML format and can be defined in four
different places. In order of increasing precedence these are:

-   the `default.yaml` file that is read from the selected resource pack
-   the mandatory `decker.yaml` file that is read from the project's root
    directory
-   the `-m key=value` options on the `decker` command line
-   additional meta data files specified in the meta data variable `meta-data`
-   the meta data sections of the slide source Markdown file

Meta data is hierarchical but most variables are defined at the top level. A
notable exception are variables that are used to set *local path* values (see
[Local paths](#local-paths)) in the slide template (see [Variables for
Reveal.js](#variables-revealjs)). These path values are located in the
`template` namespace. For example, the value for the optional title teaser image
is provided in the variable `template.title-teaser`.

Inside a YAML file or a YAML section of a file hierarchical meta variable values
are defined as follows:

``` yaml
template:
    title-teaser: /images/teaser.png
```

On the command line this can be specified as

``` sh
decker -m 'template.title-teaser=/images/teaser.pn'
```

## Local paths {#local-paths}

Paths to local file resources that are referenced by slide sets need to be
provided in several contexts. For example

-   as a URL in an image tag to locate local media files like images or videos

    ``` markdown
    ## A very important image
    ![](image.png)
    ```

-   as the value of meta data variable, for example to provide the location of
    the bibliography database and the citation style definition

    ``` yaml
    bibliography: /bib/bibliography.tex
    csl: /bib/chicago-author-data.csl
    ```

In any case, path values for local file resources are interpreted either as
relative to the defining file, if specified as a *relative path*, or as relative
to the project root directory, if specified as an *absolute path*.

Consider the following project layout and file contents

``` txt
project
├── images
│   └── image.png
└── slides
    └── slide-deck.md
```

`slides/slide-deck.md` contains:

``` markdown
# First slide
![Project relative path](/images/image.png)
![Document relative path](../images/image.png)
```

Both image paths reference the same image file.

## Variables for Reveal.js {#variables-revealjs}

Decker uses a modified version of the standard pandoc template for reveal.js
slide sets. Most if the variables used there are supported by decker.
Additionally, there are several Decker specific variables that control various
aspects of the generated slide sets.

`align-global`
:   default alignment for various slide elements (defaults to `left`)

`template.base-css` 🚧
:   the first CSS file that is loaded by the template (defaults to `''`)

`template.css` 🚧
:   a list of CSS files that is loaded after the default CSS files (defaults to
    `[]`)

`template.title-header`
:   a header image for the title slide (defaults to `''`)

`template.title-teaser`
:   an image that is placed below the title line on the title slide (defaults to
    `''`)

`template.affiliation-logo`
:   an imge that is placed above the affiliation information on the title slide
    (defaults to `''`)

`template.include-js` 🚧
:   a list of Javascript files that are included into the slide deck before
    Reveal.js is initialized (defaults to `[]`)

`style` 🚧

:   a list of CSS styles that are inserted into the HTML header (defaults to
    `[]`)

    For example, to set the background color of a all H2 header elements to red
    specify:

    ``` yaml
    style:
      - 'h2 { backgroundColor: #f00; }' 
    ```

`thebelab.enable` 🚧
:   enable ThebeLab for the deck (defaults to `false`, see
    [ThebeLab](#thebelab))

`checkOverflow`
:   mark overrflowing slides with a red border (defaults to `false`)

`vertical-slides`
:   allow vertical slides (defaults to `true`)

### Dictionary

Decker has some content that can be adapted to the language of the presentation.
This is a work-in-progress and is currently used for quizzes.

The current default dictionary looks like this:

    dictionary:
      de: 
        quiz:
          solution: Lösung zeigen
          input-placeholder: Eingeben und 'Enter'
          qmi-drag-hint: Objekte per Drag&Drop ziehen…
          qmi-drop-hint: …und hier in die richtige Kategorie einsortieren.
          ic-placeholder: Option auswählen…
      en:
        quiz:
          solution: Show Solution
          input-placeholder: Type and press 'Enter'
          qmi-drag-hint: Drag items from here…
          qmi-drop-hint: …and put them here into the correct category.
          ic-placeholder: Select option…

This dictionary can be partially or completely redefined by the user.

# Decker's Markdown

## Media handling

External media files like images or movies can be included in a presentation in
a variety of ways. The central mechanism is the standard Markdown inline image
tag as used by Pandoc.

``` markdown
![Image caption](/path/to/image.ext){width="100%"}
```

Several parameters describing the image can be encoded:

`[Image caption]`

:   If the image caption inside the square brackets `[]` is provided, the image
    will be set with the caption text right below the image. The caption text
    may contain further Markdown markup.

    A caption can also be specified by beginning the immediately following
    paragraph with the string `Caption:`. The rest of the paragraphs text is
    used as the caption.

`(/path/to/image.ext)`
:   The image itself is referenced with an URL inside the round brackets `()`. A
    relativ reference (as described in [RFC
    3986](https://tools.ietf.org/html/rfc3986#section-4.2)) here is interpreted
    as a path to a resource in the local file system that is either specified
    relative to the project's root directory or relative to the file containing
    the image tag.

`.ext`
:   The filename extension determines the media type of the image. Depending on
    the extension and media type the referenced resource may further be
    processed by decker to generate the final embedded media element.

`{width="100%"}`
:   The attributes annotation can be used to control various aspects of
    processing and presentation for the image, for example the width of the
    image relative to it's surrounding element (see [Local
    Paths](#local-paths)).

### Figures and captions

Embedded media will be rendered as a figure with caption if either

-   the square brackets of the image tag contain a caption text.

    ``` markdown
    ![This is the caption text.](some/image.png)
    ```

-   Or the image tag occurs on an otherwise empty paragraph followed directly by
    another paragraph that starts with the string `Caption:`. The second
    paragraph provides the text for the caption.

    ``` markdown
    ![](some/image.png)

    Caption: This is the caption text.
    ```

### Images

### Code blocks

Source code snippets can be included from an external file by either using the
image tag with a the `code` class or by using a Pandoc code block.

An example for a Javascript including image tag:

``` markdown
![Some Javascript code](source/code.js){.code .javascript} 
```

Standard Pandoc code blocks also work as expected:

```` markdown
``` javascript
let lork = () => {"lorgel"};
```
````

Syntax highlighting is either handled by Pandoc directly or using
[highlight.js](https://highlightjs.org). Two meta data variables control syntax
highlighting:

`highlightjs: <theme>`
:   If a theme is specified like this, highlight.js is used to perform syntax
    highlighting at load time. The theme `<theme>` is used. Pandoc does not
    process the contents of the code block.

`highlight-style: <theme>`
:   If `highlightjs` is not set, Pandoc processes the code block and emits
    highlighted spans for the content. The theme `<theme>` is used. If `<theme>`
    is invalid, the `monochrome` theme is used.

### Pdfs

### 3D polygonal models

### Iframes

### Videos

### Video streams

### Audio

### Graphs and diagrams

### Javascript ES6 modules

## Slide layout

## Whiteboard

## Audience Feedback

The audience of a deck can annotate slides with feedback. The feedback is
aggregated on a server and are visible by all audience members and the author.

The slide author can later choose to address the feedback by changing or
extending the information in the deck.

To enable this feature a deck must specify the URL of a Decker Engine server in
the meta data by setting the variable `decker-engine.base-url`. For example:

``` yaml
feedback:
  base-url: 'https://tramberend.bht-berlin.de/decker'
```

### Endpoints with authorization

There are two modes of operation depending on the deployment details of the
server, *authorized* and *public*.

If the server is running behind a proxy with Basic Authentication enabled,
questions can only be added if the user has been authenticated by the proxy.
Administrators are recognized automatically, no further authorization is
necessary.

The `de-api` endpoint works that way:

``` yaml
feedback:
  base-url: 'https://tramberend.bht-berlin.de/de-api'
```

### Public endpoints

If the server is publicly available without authentication, the user is assigned
a token that allows her to later delete or edit all questions that where added
using that token. The token can be entered by hand or stored in the browser's
local storage. Administrators need to authenticate with a username and a
password.

The `decker` endpoint works that way:

``` yaml
feedback:
  base-url: 'https://tramberend.bht-berlin.de/decker'
```

### Admistrators

Users that are authorized as administrators can answer, edit or delete all
questions in a set.

### Deck Identification

Decks are identified by their public URL. This can be problematic if a deck is
served locally, for example from
`http://localhost:8888/test/decks/engine-deck.html` during video recording, but
is supposed to show the questions on the published version. For this situation
the public URL of a deck can be set in the meta data.

``` yaml
feedback:
  deck-id: 'https://tramberend.bht-berlin.de/public/decker/test/decks/engine-deck.html'
```

If `decker-engine.deck-id` is specified, it overrides the actual deck URL as far
as deck identification for decker engine is concerned. The questions shown if
the deck is served locally will be the questions that where added to the
published deck.

## Quizzes

### Class definition

For each question type you can use either of the three tags to create quizzes

    .quiz-match-items, .quiz-mi, .qmi

    .quiz-multiple-choice, .quiz-mc, .qmc

    .quiz-insert-choices, .quiz-ic, .qic 

    .quiz-free-text, .quiz-ft, .qft

### Basic syntax

The quiz syntax is based on the markdown task list syntax. A markdown task list
looks like this

    - [ ] This box is not checked
    - [X] This box is checked
    - [ ] Another unchecked box

Questions are defined by level 2 headers. That means creating a question
**needs**

    ## Question title {.qmc}

(where `.qmc` can be replaced by any of the other quiz classes)

You can add tooltips by creating a nested list e.g.

    - [ ] A
      - tooltip A
    - [X] B
      - tooltip B

### Fenced Divs Syntax

Alternatively, quizzes can be defined using the **fenced divs** syntax:

    ::: qmc
    - [ ] A
      - tooltip A
    - [X] B
      - tooltip B
    :::

### Matching Questions

These questions generate quizzes where a user can drag and drop items to sort
them into "buckets".

This uses the Pandoc [definition list
syntax](https://pandoc.org/MANUAL.html#definition-lists).

You can provide distractor items (items not belonging to any bucket) or empty
buckets (no item belonging in those empty buckets) by using the exclamation mark
"!".

    ## Matching Question {.qmi}

    Question text

    BucketA
    : A1
    : A2

    BucketB
    : B1

    !
    : Distractor

    Empty Bucket
    : !

### Multiple Choice Questions

Classic multiple choice questions

    ## Multiple Choice Question {.qmc}

    Question text

    - [ ] A
      - nope
    - [X] B
      - yes

### InsertChoices Questions

This will create a sort of blank text questions. If multiple items are provided
in the task list, they will be rendered as a drop down menu where the user can
click answers.

If only one item/solution is provided it will be rendered as a blank.

    ## Insert Choices Question {.qic}

    - [X] A
      - of course
    - [ ] B 
      - uhm ...

    is the first letter in the ABC. The second one is

    - [ ] B
      - yep

### FreeText questions

This will create a simple input field/text box where the user can write their
answer.

    ## FreeText Question TL {.qft}

    What's the first letter in the alphabet?

    - A
      - yep
    - B
      - nope

    ## {.qft}

    What's the fourth letter?

    - [ ] C
    - [X] D

### Quiz Styling

The default style of quizzes includes decorative and interactive features. To
switch to a plain style, specify in YAML metadata, or use the `.plain` tag in
the question header.

``` .yaml
quiz: 
  style: plain
```

    # Question 1

    ## {.qmc .plain}

### Quiz Meta

Add a `YAML` code block to a question to provide meta information on the
specific question.

This is work in progress. Currently apart from `lang: de` or `lang: en` and quiz
style, it does not do anything. (21. Jul 2020)

    ``` {.yaml}
    lang: de
    score: 5
    category: FP
    lectureId: fp1
    topic: Functional Programming Introduction
    quiz:
      style: plain
    ```

## ThebeLab 🚧 {#thebelab}

## Sage

## GraphViz

## Gnuplot

# Hacking on Decker

## Conventions
