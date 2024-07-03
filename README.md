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
