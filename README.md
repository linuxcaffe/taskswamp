What is this?
=============

Taskswamp is a scriptlet that creates a `tmux` session with multiple `task`
reports and allows those reports to be easily refreshed.  Each tmux window
shows a single taskwarrior report.

Quick start
===========

To start, create a config file in `~/.taskswamprc`, following the example
below, and then run `./taskswamp`:

    % cat >> ~/.taskswamprc <<'EOF'
    { "taskswamp": -1,
      "windows": [
        { "name": "main", "task": "" },
        { "name": "waiting", "task": "waiting" },
        { "name": "myproject", "task": "project:myproject due:today ls limit:page" }
      ]
    }
    EOF
    % ./taskswamp

The config file format is json.  The `taskswamp` key must be set to `-1` (this
is a format number for forward compatibility).  The `windows` key must be set
to an array of window specifications, where each specification contains
a `name` (shown in the tmux status bar) and a `task` report to show.
The value of the `task` key is a task command-line, as a single string.
(If you type at the shell prompt `task foo bar baz`, you would put `"task":
"foo bar baz"` in the window specification.)

It is also possible to specify arbitrary commands using a `cmd` key, via
`"cmd": "echo hello world"` or `"cmd": ["echo", "hello", "world"]`.

FAQ
===

How do I refresh the task report?
---------------------------------

Press any key except `y`.  (If you press `y`, the last display will remain,
but will not be updated until you use the tmux `respawnw` colon command.)

Why doesn't `taskswamp` have a usage message, friendly error messages, etc?
---------------------------------------------------------------------------

This is just a quick ~/bin script that a handful of people use.  Making it
nicer is a SMOP that has never gotten to the top of our todo lists (for me,
it's the last item in window #3).

That said, patches _are_ welcome; email the author or open a GitHub issue.

Dependencies
------------

Required: Python 3; tmux; zsh.

Is `screen` supported?
----------------------

No, but such support would be easy to add.  `screen` can create a detached
session with multiple windows:

    screen -dmS $sessionname
    screen -S $sessionname -X screen $cmd
    ...
    screen -r $sessioname

It's just a matter of writing the glue code to invoke `screen` and to choose
between screen and tmux.
