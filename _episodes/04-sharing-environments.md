---
title: "Sharing Environments"
teaching: 30
exercises: 15
questions:
- "Why should I share my Conda environment with others?"
- "How do I share my Conda environment with others?"
- "How do I create an environment file that can be read by Windows, Mac OS, or Linux."
- "How do I specifying the package version in a Conda environment file."
objectives:
- "Understand why you would create an Conda environment file."
- "Create a Conda environment file in a text editor, specifying the channel, packages and their version."
- "Use the `conda env` subcommand to export a given environment to a environment file."
keypoints:
- "Sharing Conda environments with other researchers facilitates the reproducibility of your research."
- "Conda environment files ,`environment.yml`, describes your project's software environment."

---

## Reproducible research

Conda environments are useful when making bioinformatics projects reproducible. Full reproducibility requires the ability to recreate the system that was originally used to generate the results. This can, to a large extent, be accomplished by using a Conda environment file to make an environment with specific versions of the packages that are needed in the project. This environment file can then be shared with others users to reproduce your analysis environment containing software with the same version number.

### Creating an environment file

Conda uses YAML ("YAML Ain't Markup Language") for writing its environment files. [YAML](https://docs.ansible.com/YAMLSyntax.html) is a human-readable language that is commonly used for configuration files and that that uses Python-style indentation to indicate nesting.

Creating your project's Conda environment from a single environment file is a Conda "best practice". Not only do you have a file to share with collaborators but you also have a file that can be placed under version control which further enhancing the reproducibility of your research project and workflow.

> ## Default `environment.yml` file
>
> Note that by convention Conda environment files are called `environment.yml`. As such if you use
> the `conda env create` sub-command without passing the `--file` option, then `conda` will expect to
> find a file called `environment.yml` in the current working directory and will throw an error if a
> file with that name can not be found.
{: .callout}

Let's take a look at an example `environment.yml` file  to give you an idea of how to write your own environment files.

~~~
name: rnaseq-env
channels:
  - conda-forge
  - bioconda
  - defaults
dependencies:
  - salmon
  - fastqc
  - multiqc
~~~
{: .language-yaml}

The first line specifies a default name `rnaseq-env` for the environment, however this can be overidden on the command line. The second line specifies a list of channels, listed in priority order, that packages may need to be installed from. Finally the dependencies lists the most current and mutually compatible versions of the listed packages (including all required dependencies) to download.

The newly created environment would be installed inside the conda environment directory e.g. `~/miniconda3/envs/` directory, unless we specified a different path using `conda create` command line option `--prefix` or `-p`.

Since explicit versions numbers for all packages should be preferred a better environment file would be the following.

~~~
name: rnaseq-env
channels:
  - conda-forge
  - bioconda
dependencies:
  - salmon=1.5
  - fastqc=0.11
  - multiqc=1.10
~~~
{: .language-yaml}

Note that we are only specifying the major and minor version numbers and not the patch or build numbers. Defining the version number by fixing only the major and minor version numbers while allowing the patch version number to vary allows us to use our environment file to update our environment to get any bug fixes whilst still maintaining significant consistency of our Conda environment across updates.


> ## *Always* version control your `environment.yml` files!
>
> While you should *never* version control the contents of your `env/` environment sub-directory,
> you should *always* version control your `environment.yml` files. Version controlling your
> `environment.yml` files together with your project's source code means that you always know
> which versions of which packages were used to generate your results at any particular point in
> time.
{: .callout}

Let's suppose that you want to use the `environment.yml` file defined above to create a Conda environment in a sub-directory a project directory. Here is how you would accomplish this task.

~~~
$ cd ~/
$ mkdir rnaseq-project-2
$ cd rnaseq-project-2
~~~

Once your project folder is created, create an `environment.yml` file using your favourite editor for instance `nano`.

~~~
name: rnaseq-env
channels:
  - conda-forge
  - bioconda
dependencies:
  - salmon=1.5
  - fastqc=0.11
  - multiqc=1.10
~~~
{: .language-yaml}

Finally create a new conda environment:

~~~
$ conda env create --prefix ./env --file environment.yml
$ conda activate ./env
~~~
{: .language-bash}

Note that the above sequence of commands assumes that the `environment.yml` file is stored within your ` rnaseq-project-2` directory.

## Automatically generate an `environment.yml`

We can automatically generate the contents of an environment file using the `conda env export` command. To export the packages installed into the previously created `rnaseq-env` you can run the following command:

~~~
$ conda env export --name basic-rnaseq-env
~~~
{: .language-bash}

When you run this command, you will see the resulting YAML formatted representation of your Conda environment streamed to the terminal. Recall that we only listed three packages when we originally created `basic-rnaseq-env` yet from the output of the `conda env export` command we see that these  packages result in an environment with a large number of dependencies!

To export this list into an `environment.yml` file, you can use `--file` option to directly save the resulting YAML environment into a file.

~~~
$ conda env export --name basic-rnaseq-env --file environment.yml
~~~
{: .language-bash}

Make sure you do not have any other `environment.yml` file from before in the same directory when running the above command.

This exported environment file will however not *consistently* produce environments that are reproducible across Mac OS, Windows, and Linux. The reason is, that it may include operating system specific low-level packages which cannot be used by other operating systems.

If you need an environment file that can produce environments that are reproducible across Mac OS, Windows, and Linux, then you are better off just including those packages into the environment file that your have specifically installed using the `--from-history` option.

~~~
$ conda env export --name basic-rnaseq-env --from-history --file environment.yml
~~~
{: .language-bash}

In short: to make sure others can reproduce your environment independent of the operating system they use, make sure to add the `--from-history` argument to the `conda env export` command.

> ## Create a new environment from a YAML file.
>
> Create a new project directory rnaseq-project-3 and then create a new `environment.yml` file inside your project
> directory with the following contents.
>
> ~~~
> name: rnaseq-project3-env
> channels:
>   - conda-forge
>   - bioconda
> dependencies:
>   - salmon=1.5
>   - fastqc=0.11
>   - multiqc=1.10
> ~~~
> {: .language-yaml}
>
> Now use this file to create a new Conda environment. Where is this new environment created?
> Using the same `environment.yml` file create a Conda environment as a sub-directory called
> `env/` inside a newly created project directory. Compare the contents of the two environments.
>
> > ## Solution
> >
> > To create a new environment from a YAML file use the `conda env create` sub-command as follows.
> >
> > ~~~
> > $ cd ~/
> > $ mkdir rnaseq-project-3
> > $ cd rnaseq-project-3
> > $ nano environment.yml
> > $ conda env create --file environment.yml
> > ~~~
> > {: .language-bash}
> >
> > The above sequence of commands will create a new Conda environment inside the
> > `envs_dirs` directory. In order to create the Conda environment inside a sub-directory
> > of the project directory you need to pass the `--prefix` to the `conda env create` command as
> > follows.
> >
> > ~~~
> > $ conda env create --file environment.yml --prefix ./env
> > ~~~
> > {: .language-bash}
> >
> > You can now run the `conda env list` command and see that these two environments have been
> > created in different locations but contain the same packages.
> {: .solution}
{: .challenge}

### Updating an environment

You are unlikely to know ahead of time which packages (and version numbers!) you will need to use for your research project. For example it may be the case that  

*   one of your core dependencies just released a new version (dependency version number update).
*   you need an additional package for data analysis (add a new dependency).
*   you have found a better visualization package and no longer need to old visualization package (add new dependency and remove old dependency).

If any of these occurs during the course of your research project, all you need to do is update the contents of your `environment.yml` file accordingly and then run the following command.

~~~
$ cd
$ cd rnaseq-project-2
$ conda env update --prefix ./env --file environment.yml  --prune
~~~
{: .language-bash}

Note that the `--prune` option tells Conda to remove any installed packages not defined in environment.yml

> ## Rebuilding a Conda environment from scratch
>
> When working with `environment.yml` files it is often just as easy to rebuild the Conda
> environment from scratch whenever you need to add or remove dependencies. To rebuild a Conda
> environment from scratch you can pass the `--force` option to the `conda env create` command
> which will remove any existing environment directory before rebuilding it using the provided
> environment file.
> ~~~
> $ conda env create --prefix ./env --file environment.yml --force
> ~~~
> {: .language-bash}
{: .callout}

> ## Update environment from environment.yml
>
> Update the environment file from the previous exercise, rnaseq-project-3, by adding the package [kallisto=0.46](kallisto://pachterlab.github.io/kallisto/) and removing the salmon package. Then rebuild the environment.
>
> > ## Solution
> >
> > The `environment.yml` file should now look as follows.
> >
> > ~~~
> name: rnaseq-env
> channels:
>   - conda-forge
>   - bioconda
> dependencies:
>   - fastqc=0.11
>   - multiqc=1.10
>   - kallisto=0.46
> > ~~~
> >
> > You could use the following command, that will rebuild the environment from scratch with the
> > new dependencies:
> >
> > ~~~
> > $ cd ~/rnaseq-project-3
> > $ conda env create --prefix ./env --file environment.yml --force
> > ~~~
> > {: .language-bash}
> >
> > Or, if you just want to update the environment in-place with the new kallisto dependencies, you can use:
> >
> > ~~~
> > $ conda env update --prefix ./env --file environment.yml  --prune
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

## Restoring an environment

Conda keeps a history of all the changes made to your environment, so you can easily "roll back" to a previous version. To list the history of each change to the current environment:

~~~
$ conda activate basic-rnaseq-env
$ conda list --revisions
~~~
{: .language-bash}

To restore environment to a previous revision:

~~~
$conda install --revision=REVNUM or conda install --rev REVNUM.
~~~
{: .language-bash}

For example,

~~~
$ conda install --revision=1
~~~
{: .language-bash}

> ## List revisions.
>
> Activate the environment inside the `rnaseq-project-3` and list the revisions
> > ## Solution
> >
> > To create a new environment from a YAML file use the `conda env create` sub-command as follows.
> >
> > ~~~
> > $ cd ~/
> > $ cd rnaseq-project-3
> > $ conda activate ./env
> > $ conda list --revisions
> > ~~~
> {: .solution}
{: .challenge}

{% include links.md %}
