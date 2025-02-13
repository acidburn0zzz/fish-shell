.. _cmd-abbr:

abbr - manage fish abbreviations
================================

Synopsis
--------

| ``abbr`` --add [*SCOPE*] *WORD* *EXPANSION*
| ``abbr`` --erase *WORD* ...
| ``abbr`` --rename [*SCOPE*] *OLD_WORD* *NEW_WORD*
| ``abbr`` --show
| ``abbr`` --list
| ``abbr`` --query *WORD* ...

Description
-----------

``abbr`` manages abbreviations - user-defined words that are replaced with longer phrases after they are entered.

For example, a frequently-run command like ``git checkout`` can be abbreviated to ``gco``.
After entering ``gco`` and pressing :kbd:`Space` or :kbd:`Enter`, the full text ``git checkout`` will appear in the command line.

Options
-------

The following options are available:

**-a** *WORD* *EXPANSION* or **--add** *WORD* *EXPANSION*
    Adds a new abbreviation, causing *WORD* to be expanded to *EXPANSION*

**-r** *OLD_WORD* *NEW_WORD* or **--rename** *OLD_WORD* *NEW_WORD*
    Renames an abbreviation, from *OLD_WORD* to *NEW_WORD*

**-s** or **--show**
    Show all abbreviations in a manner suitable for import and export

**-l** or **--list**
    Lists all abbreviated words

**-e** *WORD* or **--erase** *WORD*...
    Erase the given abbreviations

**-q** or **--query**
    Return 0 (true) if one of the *WORD* is an abbreviation.

In addition, when adding or renaming abbreviations:

**-g** or **--global**
    Use a global variable

**-U** or **--universal**
    Use a universal variable (default)

See the "Internals" section for more on them.

Examples
--------

::

    abbr -a -g gco git checkout

Add a new abbreviation where ``gco`` will be replaced with ``git checkout`` global to the current shell.
This abbreviation will not be automatically visible to other shells unless the same command is run in those shells (such as when executing the commands in config.fish).

::

    abbr -a -U l less

Add a new abbreviation where ``l`` will be replaced with ``less`` universal to all shells.
Note that you omit the **-U** since it is the default.

::

    abbr -r gco gch

Renames an existing abbreviation from ``gco`` to ``gch``.

::

    abbr -e gco

Erase the ``gco`` abbreviation.

::

    ssh another_host abbr -s | source

Import the abbreviations defined on another_host over SSH.

Internals
---------
Each abbreviation is stored in its own global or universal variable.
The name consists of the prefix ``_fish_abbr_`` followed by the WORD after being transformed by ``string escape style=var``.
The WORD cannot contain a space but all other characters are legal.

Abbreviations created with the **--universal** flag will be visible to other fish sessions, whilst **--global** will be limited to the current session.
