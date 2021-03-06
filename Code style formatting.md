# Code formatting tools

## C/C++ style formatting using clang-format

Currently we use the Chromium code style formatting with a maximum line length of 100 characters.

1.  Install `clang-tools`using pacman

    ```console
    # pacman -S clang
    ```

2.  Create default style configuration in home directory

    _/home/sailbot/.clang-format_

        #BasedOnStyle: LLVM
        #BasedOnStyle: Google
        BasedOnStyle: Chromium
        #BasedOnStyle: Mozilla
        #BasedOnStyle: Webkit
        #UseTab: Always
        IndentWidth: 4
        TabWidth: 4
        ColumnLimit: 100
        ---
        Language: Cpp
        Standard: Cpp11
        # Force pointers to the type for C++.
        #DerivePointerAlignment: false
        #PointerAlignment: Left

3.  Test formatting using CLI (command line interface)

    ```console
    $ clang-format -i <SOURCEFILENAME> # Note this changes the file inplace!
    ```

    You can also format all headers and C++ sources recursively using the commandline (but you might want to make sure you are in the right directory and being very careful)

    ```console
    $ find . -iname '*.cpp' -or -iname '*.h' -exec clang-format --verbose -i {} \+
    ```

4.  Configure your editor

    ### Atom

    #### Install clang-format plugin in Atom

    -   Menu: _Edit_ -> _Preferences_ -> _Packages_ -> _+Install_ -> _Search_
    -   Search for _clang-format_ and install it
    -   _References: <https://github.com/LiquidHelium/atom-clang-format>_

    #### How to use clang-format plugin in Atom

    -   Menu: _Packages_ -> _Clang Format_ -> _Format_
    -   Keyboard shortcut: <kbd>Shift</kbd>+<kbd>K</kbd>

    ### Gedit

    #### How to install clang-format plugin in Gedit

    ```console
    $ cd /tmp
    $ git clone https://github.com/jtlatlik/gedit-plugin-clang-format.git
    $ mkdir -p ~/.local/share/gedit/plugins
    $ cp /tmp/gedit-plugin-clang-format/clangformat.* ~/.local/share/gedit/plugins/
    ```

    -   Then enable the plugin in gedit by using _Menu_ -> _Preferences_ -> _Plugins_ and enable _clang-format-plugin_

    #### How to use clang-format in Gedit

    -   <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>F</kbd>

    ### Kate

    #### How to use clang-format as a text filter in Kate

    -   Menu: _Settings_ -> _Configure Kate_ -> _Plugins_
    -   Enable _Text Filter_ and Apply

    #### How to use clang-format as a text filter in Kate

    -   Select lines you want to format (or <kbd>Ctrl</kbd>+<kbd>A</kbd> for all text)
    -   Menu: _Tools_ -> _Filter Text_ (or <kbd>Ctrl</kbd>+<kbd>Altgr</kbd>+<kbd>\\</kbd>
    -   Enter _clang-format_ as your filter (or select it in the dropdown if you have used it before)

    ### Sublime

    #### Enable clang-format in Sublime

    -   Install package control: Menu: _Tools_ -> _Install Package Control_
    -   Menu: _Preferences_ -> _Package Control_ (or press: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>)
    -   Select: _Package Control: Install Package_
    -   Search: _Clang Format_
    -   Install it
    -   Menu: _Preferences_ -> _Package settings_ -> _Clang Format_ -> _Settings - Default_
    -   Copy the contents
    -   Menu: _Preferences_ -> _Package settings_ -> _Clang Format_ -> _Settings - User_
    -   Paste
    -   Change "style": "Google" to "style": "File",

    #### How to use clang-format plugin in Sublime

    -   <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>A</kbd>

# Notes on excluding/hiding code from the formatter

If you for any reason need the clang-formatter to leave a code block untouched you can wrap it inside a block starting with `// clang-format off` and ending with `// clang-format on` like so:

```cpp
// clang-format off

std::cout <<    "This line will be left alone\n";

// clang-format on
```

# Notes on formatting of other languages

## HTML and CSS, Javascript, JSON

Install _[js-beautify](https://aur.archlinux.org/packages/js-beautify/)_ from the AUR. The package includes _html-beautify_, _css-beautify_. It might be worth mentioning you also have _tidy_ for HTML/XHTML/HTML, _fixjson_ for relaxed JSON5 and _eslint_ for Javascript.

## PHP

To use PSR2 code formatting install _[php-codesniffer](https://aur.archlinux.org/packages/php-codesniffer/) to get _phpcb_, PHP Code Beautifier. Dennis vill att du använder PSR2 med `phpcbf --standard=PSR2`.

## Shell scripts

Install _[shfmt](https://aur.archlinux.org/packages/shfmt/)_ from AUR and _[shellcheck](https://www.archlinux.org/packages/community/x86_64/shellcheck/)_ from regular repositories (`pacman -S shellcheck`). Try to follow POSIX standards and avoid relying on Bash where possible.

## Markdown

Here we install _remark_ globally using npm. Installing it for the current user only does not seem possible. Reference: https://github.com/remarkjs/remark

```console
# npm install -g remark-cli
```

## Perl

Not sure this is even used in the project but look for _perltidy_.

## Python code style formatting using autopep8

```console
# pacman -S autopep8
```

Plugins exists for Atom, Sublime and others and the executable can be used as a text filter in Kate or run straight off the commandline. Alternative formatters exists such as _yapf_. 

## Notes regarding vim

One plugin that seems to work well (for all formatters) is [vim-autoformat](https://github.com/Chiel92/vim-autoformat)
_/home/sailbot/.vimrc_

    Plug 'Chiel92/vim-autoformat'
