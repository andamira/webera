# webera

a handy shell script for the generation of static websites.

[![version: 0.1.X](https://img.shields.io/badge/version-0.1.X-d8ad4c.svg?style=flat-square)](#status)
[![language: bash](https://img.shields.io/badge/language-bash-447799.svg?style=flat-square)](https://github.com/andamira/webera/wiki/FAQ#why-bash-and-not-other_language--)
[![Build Status](https://img.shields.io/travis/andamira/webera/master.svg)](https://travis-ci.org/andamira/webera)
[![Code Climate](https://img.shields.io/codeclimate/github/andamira/webera.svg)](https://codeclimate.com/github/andamira/webera)

---

### Table of Contents

- [Features](#features-)
- [Dependencies](#dependencies-)
- [Quick Start](#quick-start-)
- [Documentation](#documentation-)
  - [Config](#config-)
  - [Usage](#usage-)
- [Reasons](#reasons-)
- [Status](#status-)

---


## Features [▴](#table-of-contents "Back to TOC")

- A configurable system for processing content templates and
  static resources, allowing custom commands and workflows.
- Content template directives allowing nesting templates,
  setting variables, displaying the output of commands...
- Includes a versatile logging system.
- Supports automatic preview.


## Dependencies [▴](#table-of-contents "Back to TOC")

Bash >=4, coreutils, sed, awk, grep...

See the detailed [list of dependencies in the wiki](https://github.com/andamira/webera/wiki/Dependencies).


## Quick Start [▴](#table-of-contents "Back to TOC")

The simplest way to start is to download the
[script](https://raw.githubusercontent.com/andamira/webera/master/webera)
and make it executable.

```sh
wget raw.githubusercontent.com/andamira/webera/master/webera && chmod +x webera
```

1) You can generate a new config file (`webera -n`) or
[download](https://raw.githubusercontent.com/andamira/webera/master/.weberarc)
the model from the repo.

2) Place your HTML templates in the `tem/` directory, and configure the
corresponding routes in the new config file, like this:

```sh
template : route : my-index.html   : /
template : route : other-page.html : /other-url/
```

3) Put your stylesheets under `res/css/` and process them like this:

```sh
resource : copy : css/ : css/
```

4) Finally, generate the website by processing the templates and resources.

```sh
$ ./webera -tr

```

The website will get generated in the `out/` directory by default:

```sh
$ find out/

  out/
  out/index.html
  out/other-url/index.html
  out/res/css/style.css
```


## Documentation [▴](#table-of-contents "Back to TOC")

The [webera Wiki](https://github.com/andamira/webera/wiki) is the documentation reference.

This project includes several complete examples you can examine for ideas *(WIP)*:

- The sources are in the [examples/](https://github.com/andamira/webera/tree/master/examples) directory.
- The generated output is under the [docs/](https://github.com/andamira/webera/tree/master/docs) directory.
- The websites are displayed in the [github page of the project](https://andamira.github.io/webera/examples/).


### Config [▴](#table-of-contents "Back to TOC")




This is a brief but intense example of a project configuration file. It shows how to:

- configure the options.
- define commands for later use on the indicated resources (or templates).
- process the resources using the previous custom command.
- process the templates, defining the routes using both file and directory URL endpoints.

```sh
# Customize Settings
config : WEB_BROWSER_BIN : chromium-browser
config : DIR_OUTPUT      : /home/$USER/my-website

# Define Custom Commands
command : sass2css : sass {ORIGIN} {TARGET}

# Process Resources
resource : sass2css : scss/styles.scss : css/main.css

# Process Templates to URL Endpoints
template : route : index.html     : /
template : route : about.html     : /about/
template : route : about-me.html  : /about/me/
template : route : about-you.html : /about/you.html
```

See [`.weberarc`](https://github.com/andamira/webera/blob/master/.weberarc) for more configuration possibilities.


### Usage [▴](#table-of-contents "Back to TOC")

There are many ways to use the script, you can combine the flags
with the configuration to achieve the behaviour you need.

In the table below you can see several examples comparing both the
short and long flags.

<table><tbody>

<tr>
  <th colspan="2">
    Usage Examples
  </th>
</tr>

<tr>
  <td colspan="2" align="left">
    <b>1.</b> Process the templates, and write a level 2 logfile,
    clearing any previous log:
  </td>
</tr>
<tr>
  <td><code>
    webera -t -cL2
  </code></td>
  <td><code>
    webera --process-templates --clear-log --log-level=2
  </code></td>
</tr>

<tr>
  <td colspan="2" align="left">
    <b>2.</b> Process templates and resources, and preview
    using a custom browser:
  </td>
</tr>
<tr>
  <td><code>
    webera -aw -W vivaldi
  </code></td>
  <td><code>
    webera --process-all --preview --browser-bin=vivaldi
  </code></td>
</tr>

<tr>
  <td colspan="2" align="left">
    <b>3.</b> Process resources <em>from</em> a custom resources directory
    <em>to</em> a custom output directory, with spaces in the path:
  </td>
</tr>
<tr>
  <td><code>
    webera -r -R resB/ -O "out B"
  </code></td>
  <td><code>
    webera --process-resources --dir-resources=resB/ --dir-output="out B"
  </code></td>
</tr>

<tr>
  <td colspan="2" align="left">
    <b>4.</b> Generate a new configuration file using the provided name,
    and write there the provided settings for the server type and port.
  </td>
</tr>
<tr>
  <td><code>
    webera -nF conf/webera.conf -S php -P 8080
  </code></td>
  <td><code>
    webera --new-config --file-config=conf/webera.conf --server-type=php --server-port=8080
  </code></td>
</tr>

</tbody></table>

Run `./webera --help` to see the basic info on flags, or visit the complete [List of flags](https://github.com/andamira/webera/wiki/Script-Arguments#list-of-flags-) in the reference wiki.


## Reasons [▴](#table-of-contents "Back to TOC")

This script was originally inspired, in its own way, by
[Statix](https://gist.github.com/plugnburn/c2f7cc3807e8934b179e),
the [Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy)
and [suckless philosophy](http://suckless.org/philosophy).

The intention is to see how far this idea can be taken,
given the limits and constraints of the shell language,
and to try to achive an ideal balance of features,
reliability, simplicity and handiness.

The script should be versatile enough to fit the needs
of both personal and professional projects.

[See the FAQ page in the wiki](https://github.com/andamira/webera/wiki/FAQ).


## Status [▴](#table-of-contents "Back to TOC")

This project is not stable yet. It is changing.
