# click-app cookiecutter template

Cookiecutter template for creating new [Click](https://click.palletsprojects.com/) command-line tools. This template has out of the box support for automatic semantic versioning using [setuptools-scm](https://setuptools-scm.readthedocs.io/) and [python-semantic-release](https://python-semantic-release.readthedocs.io/en/latest/). This repo builds on the original [click-app-template](https://github.com/simonw/click-app) by [Simon Willison](https://github.com/simonw).

Use this template on your own machine with cookiecutter, or create a brand new repository based on this template entirely through the GitHub web interface using [click-app-template-repository](https://github.com/AH-Merii/click-app-template-repository).

## Installation

You'll need to have [cookiecutter](https://cookiecutter.readthedocs.io/) installed. I recommend pipx for this:

    pipx install cookiecutter

Regular `pip` will work OK too.

## Usage

Run `cookiecutter gh:AH-Merii/click-app` and then answer the prompts. Here's an example run:

    $ cookiecutter gh:AH-Merii/click-app
    app_name []: click app template demo auto
    description []: Demonstrating https://github.com/AH-Merii/click-app
    hyphenated [click-app-template-demo-auto]: 
    underscored [click_app_template_demo]: 
    github_username []: AH-Merii
    author_name []: AbdulHamid Merii

I strongly recommend accepting the suggested value for "hyphenated" and "underscored" by hitting enter on those prompts.

This will create a directory called `click-app-template-demo-auto` - the tool name you enter is converted to lowercase and uses hyphens instead of spaces.

See https://github.com/AH-Merii/click-app-template-demo-auto for the output of this example.

## Developing your command-line tool

Having created the new structure from the template, here's how to start working on the tool.

If your tool is called `my-new-tool`, you can start working on it like so:

    cd my-new-tool
    # Create and activate a virtual environment:
    python3 -m venv venv
    source venv/bin/activate
    # Install dependencies so you can edit the project:
    pip install -e '.[test]'
    # With zsh you have to run this again for some reason:
    source venv/bin/activate
    # Confirm your tool can be run from the command-line
    my-new-tool --version

You should see the following:

    my-new-tool, version 0.0.1

You can run the default test for your tool like so:

    pytest

This will execute the test in `tests/test_my_new_tool.py`.

Now you can open the `my_new_tool/cli.py` file and start adding Click [commands and groups](https://click.palletsprojects.com/en/7.x/commands/).

## Creating a Git repository for your tool

You can initialize a Git repository for your tool like this:

    cd my-new-tool
    git init
    git add .
    git commit -m "Initial structure from template"
    # Rename the 'master' branch to 'main':
    git branch -m master main

## Publishing your tool to GitHub

Use https://github.com/new to create a new GitHub repository sharing the same name as your tool, which should be something like `my-new-tool`.

Push your `main` branch to GitHub like this:

    git remote add origin git@github.com:YOURNAME/my-new-tool.git
    git push -u origin main

The template will have created a GitHub Action which runs your tool's test suite against every commit.

## Guide to Using `python-semantic-release` with Commit Conventions for Automated Versioning

`python-semantic-release` automates the versioning and package release process based on the semantics of your commit messages. To make effective use of this tool, it's important to understand how to structure your commits for patches, minor changes, and major changes.


### Commit Conventions

To ensure `python-semantic-release` correctly bumps versions, you need to structure your commit messages according to the Conventional Commits specification.

Prefixes in commit messages provide clear and consistent categorization of changes, enabling automated tools to determine versioning and changelogs accurately.

#### No-bump Prefixes
Prefixes that Typically Don't Trigger a Version Bump:
* `chore:`: Routine tasks or maintenance chores.
* `docs:`: Documentation changes, unless they're part of the published package/module.
* `style:`: Code style changes, like formatting.
* `test:`: Changes exclusively in tests, unless they're part of the published package/module.
* `refactor:`: Code changes that neither fix a bug nor add a feature. Even though refactoring might involve significant codebase changes, it doesn't introduce new features from an end-user's perspective.
* `build:`: Changes affecting the build system or external dependencies.
* `ci:`: Adjustments to the Continuous Integration configuration or scripts.
* `revert:`: Reverting a previous commit. The version bump is determined by the nature and impact of the reverted commit.

#### Patches (Bug Fixes)

For patches or bug fixes, use the `fix` prefix:

```bash
git commit -m "fix: corrected typo in user interface"
```

This will result in a patch version bump (e.g., `1.0.0` -> `1.0.1`).

Prefixes that Trigger a Patch Bump:

* `fix:`: Bug fixes typically result in a patch version bump.
* `perf:`: Performance improvements can result in a patch version bump if they don't introduce new features.

#### Minor Changes (New Features)

For new features that are backward-compatible, use the `feat` prefix:

```bash
git commit -m "feat: added dark mode support"
```

This will lead to a minor version bump (e.g., `1.0.0` -> `1.1.0`).


#### Major Changes (Breaking Changes)

For breaking changes, you need to include a prefix in the commit title and you need to include `BREAKING CHANGE:` in the commit message body. To provide both a title and a body in the commit message from the command line, you can use the `-m` flag twice:

##### Using CLI:
```bash
git commit -m "feat: revised authentication flow" -m "BREAKING CHANGE: new authentication method, old method deprecated"
```


The first `-m` provides the title (or header) of the commit, and the second `-m` provides the body, detailing the breaking change. This structure ensures that tools following the Conventional Commits specification can correctly detect and process the breaking change.

This will trigger a major version bump (e.g., `1.0.0` -> `2.0.0`).

It is worth noting that any commit can introduce a breaking change, and if it does (indicated by the BREAKING CHANGE: notation in the commit body), it will always trigger a major version bump, regardless of the prefix used, as long as there is a prefix in the title.

## Publishing your cool as a package to PyPI

The template also includes a `publish.yml` GitHub Actions workflow for publishing packages to [PyPI](https://pypi.org/), using [pypa/gh-action-pypi-publish](https://github.com/pypa/gh-action-pypi-publish).

To use this action, you need to create a PyPI account and configure a Trusted Publisher.

Once you have created your account, navigate to https://pypi.org/manage/account/publishing/ and create a "pending publisher" for the package. Use the following values:

- **PyPI Project Name:** The name of your package
- **Owner:** Your GitHub username or organization - the "foo" in `github.com/foo/bar`
- **Repsitory name:** The name of your repository - the "bar" in `github.com/foo/bar`
- **Workflow name:** `publish.yml`
- **Environment name:** `release`

Now, any time you create a new "Release" on GitHub the Action will build your package and push it to PyPI.

The tag for your release needs to match the `VERSION` string at the top of your `pyproject.toml` file. You should bump this version any time you release a new version of your package.
