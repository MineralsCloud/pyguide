# Python environment

# [What is the difference between `venv`, `pyvenv`, `pyenv`, `virtualenv`, `virtualenvwrapper`, `pipenv`, `pipx`, etc?](https://stackoverflow.com/questions/41573587/what-is-the-difference-between-venv-pyvenv-pyenv-virtualenv-virtualenvwrappe)

## PyPI packages not in the standard library:

- **[`virtualenv`][1]** is a very popular tool that creates isolated Python
environments for Python libraries. If you're not familiar with this tool, I
highly recommend learning it, as it is a very useful tool, and I'll be making
comparisons to it for the rest of this answer.

  It works by installing a bunch of files in a directory (eg: `env/`), and then
  modifying the `PATH` environment variable to prefix it with a custom `bin`
  directory (eg: `env/bin/`). An exact copy of the `python` or `python3` binary
  is placed in this directory, but Python is programmed to look for libraries
  relative to its path first, in the environment directory. It's not part of
  Python's standard library, but is officially blessed by the PyPA (Python
  Packaging Authority). Once activated, you can install packages in the virtual
  environment using `pip`.

- **[`pyenv`][2]** is used to isolate Python versions. For example, you may
want to test your code against Python 2.6, 2.7, 3.3, 3.4 and 3.5, so you'll
need a way to switch between them. Once activated, it prefixes the `PATH`
environment variable with `~/.pyenv/shims`, where there are special files
matching the Python commands (`python`, `pip`). These are not copies of the
Python-shipped commands; they are special scripts that decide on the fly
which version of Python to run based on the `PYENV_VERSION` environment
variable, or the `.python-version` file, or the `~/.pyenv/version` file.
`pyenv` also makes the process of downloading and installing multiple Python
versions easier, using the command `pyenv install`.

- **[`pyenv-virtualenv`][3]** is a plugin for `pyenv` by the same author as
`pyenv`, to allow you to use `pyenv` and `virtualenv` at the same time
conveniently. However, if you're using Python 3.3 or later,
`pyenv-virtualenv` will try to run `python -m venv` if it is available,
instead of `virtualenv`. You can use `virtualenv` and `pyenv` together
without `pyenv-virtualenv`, if you don't want the convenience features.

- **[`virtualenvwrapper`][4]** is a set of extensions to `virtualenv` (see
[docs][5]). It gives you commands like `mkvirtualenv`, `lssitepackages`, and
especially `workon` for switching between different `virtualenv` directories.
This tool is especially useful if you want multiple `virtualenv` directories.

- **[`pyenv-virtualenvwrapper`][6]** is a plugin for `pyenv` by the same author
as `pyenv`, to conveniently integrate `virtualenvwrapper` into `pyenv`.

- **[`pipenv`][7]**, by Kenneth Reitz (the author of `requests`), is the newest
project in this list. It aims to combine `Pipfile`, `pip` and `virtualenv`
into one command on the command-line. The `virtualenv` directory typically
gets placed in `~/.local/share/virtualenvs/XXX`, with `XXX` being a hash of
the path of the project directory. This is different from `virtualenv`, where
the directory is typically in the current working directory.

  The Python Packaging Guide [recommends `pipenv`][8] when developing Python
  applications (as opposed to libraries). There does not seem to be any plans
  to support `venv` instead of `virtualenv` ([#15][9]). Confusingly, its
  command-line option `--venv` refers to the `virtualenv` directory, not
  `venv`, and similarly, the environment variable `PIPENV_VENV_IN_PROJECT`
  affects the location of the `virtualenv` directory, not `venv` directory
  ([#1919][10]).

## Standard library:

- **`pyvenv`** is a script shipped with Python 3 but [deprecated in Python
  3.6][11] as it had problems (not to mention the confusing name). In Python
  3.6+, the exact equivalent is `python3 -m venv`.

- **[`venv`][12]** is a package shipped with Python 3, which you can run using
  `python3 -m venv` (although for some reason some distros separate it out into
  a separate distro package, such as `python3-venv` on Ubuntu/Debian). It serves
  a similar purpose to `virtualenv`, and works in a very similar way, but it
  doesn't need to copy Python binaries around (except on Windows). Use this if
  you don't need to support Python 2. At the time of writing, the Python
  community seems to be happy with `virtualenv` and I haven't heard much talk of
  `venv`.

  Most of these tools complement each other. For instance, `pipenv` integrates
  `pip`, `virtualenv` and even `pyenv` if desired. The only tools that are true
  alternatives to each other here are `venv` and `virtualenv`.

## Recommendation for beginners:

This is my personal recommendation for beginners: start by learning `virtualenv`
and `pip`, tools which work with both Python 2 and 3 and in a variety of
situations, and pick up the other tools once you start needing them.

[1]: https://pypi.python.org/pypi/virtualenv
[2]: https://github.com/yyuu/pyenv
[3]: https://github.com/yyuu/pyenv-virtualenv
[4]: https://pypi.python.org/pypi/virtualenvwrapper
[5]: http://virtualenvwrapper.readthedocs.io/en/latest/
[6]: https://github.com/yyuu/pyenv-virtualenvwrapper
[7]: https://pypi.python.org/pypi/pipenv
[8]: https://packaging.python.org/guides/tool-recommendations/#application-dependency-management
[9]: https://github.com/pypa/pipenv/issues/15
[10]: https://github.com/pypa/pipenv/issues/1919
[11]: https://docs.python.org/dev/whatsnew/3.6.html#id8
[12]: https://docs.python.org/3/library/venv.html
[13]: https://docs.python.org/3/library/venv.html#creating-virtual-environments