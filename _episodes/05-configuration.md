---
title: "Configuring Conda"
teaching: 20
exercises: 5
questions:

- "How do I configure conda ?"
- "How can I see conda's configuration values?"
- "How can I modify conda's configure settings?"
objectives:
- "Use the conda `config --show` to display all configuration values and `config --describe` to describe agiven configuration parameters."
- "Modify the `.condarc` file using the `conda config` sub command."
- "locate the `.condarc` file."
keypoints:
- "The `.condarc` is an optional runtime configuration file that stores custom conda setting."
- "You can use the `conda config` subcommand to add, set or remove configuration setting in the `.condarc` file. You can also  edit the `.condarc` directly using a text editor."

---

## Configuration

Conda has a number of configuration setting which control how it performs.
To display and control these setting we can use the `config config` subcommand.

To display all configuration settings run the `config --show` subcommand :

~~~
$ conda config --show
~~~
{: .language-bash}

As you can see Conda supports a large number of configuration options.
To show a single setting add the setting name after the `conda config --show` command.
For example, to show the configurtion setting for the channels conda searches run:

~~~
$ conda config --show channels
~~~
{: .language-bash}

~~~
channels:
  - defaults
~~~
{: .output

By default conda only  searches the `defaults` channel, this is why we had to include `conda-forge` via the command line option `-c` in the previous episode.

To get more information about an individual setting run `conda config --describe <option>`. For Example;

~~~
conda config --describe channels
~~~
{: .language-bash}

~~~
# # channels (sequence: primitive)
# #   aliases: channel
# #   env var string delimiter: ','
# #   The list of conda channels to include for relevant operations.
# #
# channels:
#   - defaults
~~~
{: .output}

## .condarc

A user's conda setting are store in the runtime configuration configuration file, `.condarc`. This file  allows  users to configure various aspects of conda  including:

* Where conda looks for packages `channels`.

* Where conda lists known environments `envs_dirs`.

* Whether to update the Bash prompt with the currently activated environment name `env_prompt`.

* What default packages or features to include in new environments `create_default_packages`.

Like the environment file the `.condarc` configuration file follows a simple YAML syntax

The `.condarc file` is not included by default, but it is automatically created in your home directory the first time you run the `conda config` command.


## Creating or modify .condarc

To create or modify a `.condarc file`,  enter the `conda config` command and use the modifier options  `--add`, `--set`, `--append` , `--prepend` or `--remove` followed by the configuration key and a value .

~~~
conda config <modifier> <KEY> <VALUE>
~~~
{: .language-bash}

### Adding a configuration value

To add the `conda-forge` to the list of `channels` we can use the `--add`, `--append` or `--prepend` modifier option:

For example, if we want to add a channel to our list of `channels` in our configuration file rather than specific it on the command line every time we can can use the `--append` option modifier.

~~~
$conda config --append channels conda-forge
~~~
{: .language-bash}

To add `conda-forge` to the beginning of the channel list, giving it the highest priority use the `--preprend` modifier options.

~~~
$conda config --prepend channels conda-forge
~~~
{: .language-bash}


**Note:** It is generally best to have `conda-forge` as the highest priority channel as this will usually have the most up-to-date packages.


> ## Adding the channels bioconda and conda-forge to .condarc.
>
> Add the bioconda and conda-forge channels to  your .condarc file.
> Which channel has the highest priority?
> > ## Solution
> >
> > To add the bioconda and conda-forge channel to  your .condarc file use the command.
> >
> > ~~~
> > $ conda config --prepend channels bioconda
> > $ conda config --prepend channels conda-forge
> > ~~~
> > {: .language-bash}
> >
> > The above sequence of commands will add the channels to your .condarc
> > Use the command below to show the channel priority order.
> > ~~~
> > $ conda config --get channels
> > ~~~
> > {: .language-bash}
> >
> {: .solution}

## Setting configuration settings

If our configuration setting has a single value we can use `conda config --set` to set it.

For example, In a previous episode we set the command line prompt setting for conda using  `env_prompt`.


~~~
$ conda config --describe env_prompt
~~~
{: .language-bash}

The `env_prompt` setting takes a value of either `'{prefix}'`, `'{name}'`, and
`'{default_env}'`.

~~~
# # env_prompt (str)
# #   Template for prompt modification based on the active environment.
# #   Currently supported template variables are '{prefix}', '{name}', and
# #   '{default_env}'. '{prefix}' is the absolute path to the active
# #   environment. '{name}' is the basename of the active environment
# #   prefix. '{default_env}' holds the value of '{name}' if the active
# #   environment is a conda named environment ('-n' flag), or otherwise
# #   holds the value of '{prefix}'. Templating uses python's str.format()
# #   method.
# #
# env_prompt: '({default_env}) '
~~~
{: .output}

To set the `env_prompt` to the default value `'({default_env})'` we can run:

~~~
$ conda config --set env_prompt '({default_env})'
~~~
{: .language-bash}

To change it back to the just the environment name, we can run.

~~~
$ conda config --set env_prompt '({name})'
~~~
{: .language-bash}

**Note:**: You need to deactivate then reactivate the environment for the changes in env prompt to take effect.

## Editing the `.condarc` file manually.

You can also use a text editor such as nano to directly edit the `.condarc`.

To show the location and contents of your `.condarc` file you can use the `conda config --show-sources` command.

~~~
$ conda config --show-sources
~~~
{: .language-bash}

**Note:** If the .condarc file is in the root environment, it will override any in the home directory.

## Getting help

As with all conda commands you can use the `--help` option to get help.

For example,  for a complete list of `conda config` commands run

~~~
$ conda config --help
~~~
{: .language-bash}

Or see the [command reference](https://conda.io/projects/conda/en/latest/commands/config.html).

{% include links.md %}
