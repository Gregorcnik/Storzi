# Storži

Repository of patterns running on Jelka FMF.

## About

This repository contains all patterns that are running on [Jelka FMF](https://jelka.fmf.uni-lj.si/).

The patterns can be found in the `patterns` directory. Each pattern is in a separate
directory and can be written in most programming languages that support outputting
to the standard output and can be run in a Docker container.

All patterns from the repository are automatically compiled and deployed to the
Jelkob server and will be running on the official Christmas tree at the Faculty
of Mathematics and Physics, University of Ljubljana.

## Contributing

### General Information

All patterns are stored in the `patterns` directory.

Each pattern is contained in a separate directory that contains `config.yml` and
`main.*` files, and optionally a `Dockerfile`.

The directory name is used as a pattern ID. It should be similar to the pattern
name and should represent what the pattern does. The directory name should only
contain lowercase letters, numbers and hyphens.

The `config.yml` file contains the configuration of the pattern. It provides basic
information about the pattern, such as the name, description, author and school.

The name should be a short, descriptive and unique name of the pattern, and the
description should explain what your pattern does in a short sentence.

The `main.*` file is the main file of the pattern, which is executed when the
pattern is started.

If you are writing a pattern in one of the languages that we provide a template for,
the language will be automatically detected based on the file extension.

If you are writing a pattern in a language that we do not provide a template for,
you will also have to write a custom `Dockerfile` that will be used to compile
and run the pattern. When started, the `Dockerfile` should run the pattern and
pipe its output to `/tmp/jelka`.

If you are writing a pattern in a language that we provide a template for, you can
still write a custom `Dockerfile` if you need additional configuration, but it is
recommended to use the default template if possible.

### Adding a Pattern

First, for the repository and clone it to your computer:

```bash
git clone https://github.com/Jelka-FMF/Storzi.git
cd Storzi
```

Then, install the required dependencies:

```bash
pip install -r requirements.txt
```

To add a new pattern, create a new directory in the `patterns` directory, and add
the `config.yml` and `main.*` files. Check the above section for general information
about the files, and the language-specific sections below.

After you have added the pattern (and formatted it properly), commit your changes:

```bash
git add patterns/pattern-name
git commit -m "Your commit message"
```

You can then push your changes and create a pull request.

### Python Patterns

When writing a Python pattern, you should install the recommended libraries from
the `requirements.txt` file in the root of the repository:

```bash
pip install -r requirements.txt
```

This will install all available libraries that can be used in Python patterns, in
addition to the simulation for running the patterns locally, and development tools.

Your pattern can use the [Jelka Python API](https://github.com/Jelka-FMF/JelkaPy),
as other available libraries (see below).

The main pattern filename must be `main.py`.

While developing your pattern, you can run it locally using the simulation:

```bash
jelkasim patterns/pattern-name/main.py
```

Before commiting your pattern, please make sure it is properly formatted:

```bash
ruff check patterns/pattern-name
ruff format patterns/pattern-name
```

### JavaScript Patterns

When writing a JavaScript pattern, you should install the recommended libraries from
the `package.json` file in the root of the repository:

```bash
npm install
```

This will install all available libraries that can be used in JavaScript patterns,
in addition to development tools.

You should still install Python dependencies as specified in the above section,
as they are used for running the simulation.

Your pattern can use the [Jelka JavaScript API](https://github.com/Jelka-FMF/JelkaJS),
as other available libraries (see below).

The main pattern filename must be `main.js`.

While developing your pattern, you can run it locally using the simulation:

```bash
jelkasim node patterns/pattern-name/main.js
```

Before commiting your pattern, please make sure it is properly formatted:

```bash
npm run format patterns/pattern-name
```

### Guidelines

* Patterns should be written in a way that they can be run in a Docker container.
* Patterns should output to the standard output (which is piped into `/tmp/jelka`).
* Patterns should not display inappropriate content.

## Templates

### Python

* Base image: [`images/python`](images/python)
* Default template: [`defaults/python`](defaults/python)
* Available libraries: [`requirements.txt`](images/python/requirements.in)
* Pattern filename: `main.py`

### JavaScript

* Base image: [`images/javascript`](images/javascript)
* Default template: [`defaults/javascript`](defaults/javascript)
* Available libraries: [`package.json`](images/javascript/package.json)
* Pattern filename: `main.js`

## Structure

* `patterns/` - Directory containing all patterns
    * `pattern-name/`
        * `config.yml` - Configuration file for the pattern
        * `main.*` - Main file of the pattern

* `defaults/` - Default Docker templates for patterns
    * `javascript/` - Default Docker template for JavaScript patterns
    * `python/` - Default Docker template for Python patterns

* `images/` - Base Docker images for patterns
    * `javascript/` - Base Docker image for JavaScript patterns
    * `python/` - Base Docker image for Python patterns

## License

By contributing to this repository, you agree to license your work under the MIT license.
