# proj

A project information and validation tool.

Similar to *nix `file` command, this software devemopment tool is intended to examine the contents of a directory to determine what programming languages and frameworks are used, which tools are required to build the code and how to build the code.


## Usage

`proj` provides the following commands:
* `proj info .`: Examine the current directory for project information. This will recurse up the directory hierarchy until it matches something or fails.
* `proj check .`: Assuming . contains a project `proj` can identify, `check` will review the standard system path to confirm necessary tools are available. Any tools that aren't, print link to install docs.
* `proj build .`: Assuming all the tools are available
  * -s/--skip-check - turn off default tool check.
  * -d/--dry-run - print commands, but don't execute
  * -v, -vv   - print info and debug logs to console. errors to stderr on by default
  * -q - no console output, even errors.


## Dev Journal

Just keepin' notes about the development.

### 2024/07/03

Initial conception and setup. Intention is to codify (literally in code and and supporting data files) a means of determining information about the project. Given a project dir, examine the files and determine what tools are needed to build.

This information should countain links on how to install each tool needed.

Examining some standard file names:
```
{ FileName: "package.json", Language: "js" }
{ FileName: "Gemfile", Language: "ruby" }
{ FileName: "pom.xml", Language: "maven" }
build.xml -> ant, java
build.gradle -> gradle, java
setup.py -> py
requirements.txt -> py
go.mod -> go
cargo.toml - > rust
Makefile -> make (analyze makefile for commands -> `\t[ @]*(\S+)\s`gm  capture group(1) has commands
build.sh -> *nix - read README
configure -> *nix - read README
Dockerfile docker
docker-compose.yaml docker

prettierrc.json
tsconfig.json - typescript
*.ipynb - jupyter notebook
babel.config.js
gulpfile.js
tailwind.config.js
test2.js
vue.config.js
venv/ -> venv/py
.venv/ -> venv/py
setup.cfg
pyproject.toml
tox.ini
```

Researched golang CLI tools and libraries. Top three seem to be:
* https://github.com/spf13/cobra - provides a detailed model and tools to generate "command files". structured and directed.
* https://github.com/urfave/cli - provides solid examples for CLI parsing https://cli.urfave.org/v2/examples
* https://github.com/jessevdk/go-flags - provides structure-based annotation parsing: fill annotated option struct from cli args.

Starting implementation with cobra. docs and support seem solid.

Initial commit to github.

Tool dependency chain: each filename (regex) matchs a specific tool, which might have dependencies. Tool dependencies must be satisfied to pass a check. A failed check should disply the install info.

Data needed:
* filename (glob?) to match
* tool associates with file name
* for each tool: name, install_url, dependencies

Build big index from filename

Search algo:
For a given directory `.`:
- for each file in directory:
  - lookup the filename (and ext?) in a hash of names
  - if found, the reference is a tools name.
  - collect it in a set of tools.
- if tools is empty, `cd ..`
- when tools has info, for each tool (depends pass):
  - lookup in hash of tools.
  - if tool has dependencies, *recursively* add dependencies until there are none.
-
