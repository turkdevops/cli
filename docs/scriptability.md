# Scriptability audit

This document acts as a high level TODO list for improving scriptability of `gh`
across all of our commands.

## gh pr

### create

- [x] Behavior when `STDIN` is a pipe

  _Current_: It breaks. Because STDIN is occupied, survey fails to prompt.
  _Desired_: It doesn't break. It'd be cool if we could take PR data to pre-fill
  the prompts with but that opens up questions of how to serialize the input.
  For now it should just behave as if --fill was passed.

- [x] Behavior when `STDOUT` is piped out

  _Current_: It breaks. survey writes to the pipe and the command hangs
  indefinitely. _Desired_: It doesn't break. --fill should be assumed since it's
  impossible to prompt.

- [x] Behavior when run un-attached to a terminal

  _Current_: It hangs still attempting to prompt. _Desired_: --fill should be
  assumed.

- [x] Consistent use of `STDERR`

  _Current_: Almost all output in the non-interactive case goes to STDERR.
  _Desired_: All output _except_ the final PR URL is sent to STDERR in the
  non-interactive case.

### close

- [x] Behavior when `STDIN` is piped to command

  _Current_: Argument validation error. _Desired_: STDIN should be parsed as a
  single PR argument.

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### checkout

- [x] Behavior when `STDIN` is piped to command

  _Current_: Validation error _Desired_: STDIN parsed as a single PR argument

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### diff

- [x] Behavior when `STDIN` is piped to command

  _Current_: STDIN is ignored and current branch's PR is used, if any.
  _Desired_: STDIN is parsed as a single PR argument

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### list

- [x] Behavior when `STDIN` is piped to command
- [x] Behavior when `STDOUT` is piped out

  _Current_: Just the PR list is printed in a slightly different format
  _Desired_: The format is more machine-friendly and conveys what is lost
  without color. It should be easy to process with `cut` or `awk`.

- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

  _Current_: "Showing x of y pull requests in owner/repo" is always printed to
  STDERR _Desired_: Nothing is printed to STDERR (except warnings/errors) if
  running non-interactively

### merge

- [x] Behavior when `STDIN` is piped to command

  _Current_: Broken. Survey tries and fails to prompt user. _Desired_: If a
  pipe, STDIN should be parsed as a single PR argument and then the merge should
  run non-interactively.

- [x] Behavior when `STDOUT` is piped out

  _Current_: Broken. Survey hangs forever. _Desired_: Non-interactive is
  assumed.

- [x] Behavior when run un-attached to a terminal

  _Current_: Broken. _Desired_: Non-interactive is assumed.

- [x] Consistent use of `STDERR`

  _Current_: All output goes to STDOUT _Desired_: Output should go to STDERR

### ready

- [x] Behavior when `STDIN` is piped to command

  _Current_: STDIN is ignored _Desired_: STDIN is parsed as a single PR
  argument.

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### reopen

- [x] Behavior when `STDIN` is piped to command

  _Current_: Validation error _Desired_: STDIN is parsed as a single PR
  argument.

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### review

- [x] Behavior when `STDIN` is piped to command

  _Current_: STDIN is ignored _Desired_: STDIN is parsed as a single PR
  argument.

- [x] Behavior when `STDOUT` is piped out

  _Current_: Hangs waiting for input. _Desired_: If lacking enough flags to run
  non-interactively, clear error. Otherwise just run.

- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

  _Current_: Outputs everything to STDOUT _Desired_: Should send current output
  to STDERR but print URL of review/comment to STDOUT

### status

- [x] Behavior when `STDIN` is piped to command
- [x] Behavior when `STDOUT` is piped out

  _Current_: Same human-oriented output is printed _Desired_: `grep`/`awk`/`cut`
  friendly output

- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### view

- [x] Behavior when `STDIN` is piped to command

  _Current_: Ignored _Desired_: STDIN parsed as single PR argument

- [x] Behavior when `STDOUT` is piped out

  _Current_: Rendered markdown and human metadata written to pipe _Desired_: Raw
  markdown is printed, possibly made possible via a --raw flag. Metadata printed
  linewise for grepping.

- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

## gh issue

### close

- [x] Behavior when `STDIN` is piped to command

  _Current_: Argument validation error. _Desired_: STDIN should be parsed as a
  single PR argument.

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### create

- [x] Behavior when `STDIN` is piped to command

  _Current_: Breaks. Survey attempts to parse STDIN. _Desired_: Doesn't break
  but errors clearly saying that piping on STDIN is unsupported

- [x] Behavior when `STDOUT` is piped out

  _Current_: Hangs. Survey writing to pipe. _Desired_: Clear error

- [x] Behavior when run un-attached to a terminal

  _Current_: Hangs. _Desired_: Clear error

- [x] Consistent use of `STDERR`

### list

- [x] Behavior when `STDIN` is piped to command
- [x] Behavior when `STDOUT` is piped out

  _Current_: Just the issue list is printed in a slightly different format
  _Desired_: The format is more machine-friendly and conveys what is lost
  without color. It should be easy to process with `cut` or `awk`.

- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

  _Current_: "Showing x of y pull requests in owner/repo" is always printed to
  STDERR _Desired_: Nothing is printed to STDERR (except warnings/errors) if
  running non-interactively

### reopen

- [x] Behavior when `STDIN` is piped to command

  _Current_: Validation error _Desired_: STDIN is parsed as a single issue
  argument.

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### status

- [x] Behavior when `STDIN` is piped to command
- [x] Behavior when `STDOUT` is piped out

  _Current_: Same human-oriented output is printed _Desired_: `grep`/`awk`/`cut`
  friendly output

- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

### view

- [x] Behavior when `STDIN` is piped to command

  _Current_: Ignored _Desired_: STDIN parsed as single issue argument

- [x] Behavior when `STDOUT` is piped out

  _Current_: Rendered markdown and human metadata written to pipe _Desired_: Raw
  markdown is printed, possibly made possible via a --raw flag. Metadata printed
  linewise for grepping.

- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

## gh repo

### clone

- [x] Behavior when `STDIN` is piped to command

  _Current_: Ignored _Desired_: STDIN parsed as single repo argument

- [x] Behavior when `STDOUT` is piped out
- [x] Behavior when run un-attached to a terminal
- [x] Consistent use of `STDERR`

  _Current_: Output from git is on both STDERR and STDOUT. We print nothing
  ourselves to STDOUT. _Desired_: Git information should all be on stderr and
  potentially filtered (without a --debug or --verbose option). Only thing on
  STDOUT should be resulting URL of clone.

### create

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

### fork

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

### view

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

## gh alias

### delete

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

### list

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

### set

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

## gh api

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

## gh config

### set

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

### get

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`

## gh gist

### create

- [ ] Behavior when `STDIN` is piped to command
- [ ] Behavior when `STDOUT` is piped out
- [ ] Behavior when run un-attached to a terminal
- [ ] Consistent use of `STDERR`
