---
title: "Configuring conda"
teaching: 30
exercises: 0
questions:
- "How do I configure conda ?"

objectives:
- "modify the .condarc file."

keypoints:
- "The .condarc is an optional runtime configuration file that allows users to configure various aspects of conda"
- "You can use the `conda config` subcommand to modify configuration options."


---

## Configuration

Conda has a number of configuration setting which control how it performs.
To display and control these setting we can use the `config config` subcommand.
To display all configuration values:

~~~
conda config --show
~~~
{: .language-bash}

Conda supports a wide range of configuration options. This episode gives examples of frequently used options and their usage. For a complete list of all available options for your version of conda, use the `conda config --describe` command.

~~~
$conda config --describe
~~~
{: .language-bash}


## .condarc


The conda configuration file, `.condarc`, is an optional runtime configuration file that allows  users to configure various aspects of conda, such as which channels it searches for packages and environment directories.


The .condarc file can change many parameters, including:

* Where conda looks for packages.

* Where conda lists known environments.

* Whether to update the Bash prompt with the currently activated environment name.

* What default packages or features to include in new environments.

The .condarc configuration file follows simple YAML syntax


The `.condarc file` is not included by default, but it is automatically created in your home directory the first time you run the `conda config` command.


## Creating or modify .condarc

To create or modify a `.condarc file`, open  enter the `conda config` command and use the options  `--add`, `--set`, `--append` , `--prepend` or `--remove` .

~~~
conda config <modifier> <KEY> <VALUE>
~~~
{: .language-bash}

For example to add the `conda-forge` channel to the configuration file at the end of a list.

~~~
$conda config --add channels conda-forge
~~~
{: .language-bash}

To add conda-forge to the beginning of a list run.

~~~
$conda config --prepend channels conda-forge
~~~
{: .language-bash}


Alternatively, you can open a text editor such as Notepad on Windows, TextEdit on macOS, or VS Code. Name the new file `.condarc` and save it to your user home directory or root directory.

To edit the `.condarc` file, open it from your home or root directory and make edits in the same way you would with any other text file. If the .condarc file is in the root environment, it will override any in the home directory.

You can find information about your `.condarc` file by typing `conda info` in your terminal or Anaconda Prompt. This will give you information about your .condarc file, including where it is located.

~~~
conda info
~~~
{: .language-bash}

To set configuration options, edit the .condarc file directly or use the `conda config --set` command.

EXAMPLE: To set the env_prompt option run:

~~~
conda config --describe env_prompt
~~~

To see the options.

{: .language-bash}

~~~
conda config --set env_prompt '({name})'
~~~
{: .language-bash}

For a complete list of conda config commands, see the [command reference](https://conda.io/projects/conda/en/latest/commands/config.html). The same list is available at the terminal or Anaconda Prompt by running `conda config --help`. You can also see the conda channel configuration for more information.


## General configuration

### Channel locations (channels)

Listing channel locations in the .condarc file overrides conda defaults, causing conda to search only the channels listed here, in the order given.

Use defaults to automatically include all default channels. Non-URL channels are interpreted as Anaconda.org user names.

~~~
conda config --show channels
~~~
{: .language-bash}

### Specify environment directories (envs_dirs)

Specify directories in which named environments are located. If the envs_dirs key is set, the root prefix envs_dir is not used unless explicitly included.

To list environment directories run `conda config --show envs_dirs`.

~~~
conda config --show envs_dirs
~~~
{: .language-bash}

Conda will write environments to the first writable directory in the list.

### Specify package directories (pkgs_dirs)

Specify directories in which packages are located. If the `pkgs_dirs` key is set, the root prefix, CONDA_ROOT is the path for your base conda install , is not used unless explicitly included.

If the `pkgs_dirs key` is not set, then envs/pkgs is used as the pkgs cache, except for the standard envs directory in the root directory, for which the normal root_dir/pkgs is used.

~~~
conda config --show pkgs_dirs
~~~
{: .language-bash}

{% include links.md %}
