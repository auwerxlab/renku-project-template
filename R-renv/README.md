# {{ name }}
{% if description %}
{{ description }}
{% else %}
*Description of the project goals, applications and steps.*
{% endif %}

## Get this project

```
$ git clone <this_project_url>.git
```

## Environment

This is an R project and the ``.Rproj`` file is located in the main directory.

This project can be used on the Renku platform.

The entire environment required for this project is described in the ``Dockerfile`` located in the main directory. \
It allows to generate a docker image that runs an instance of R Studio server in which the R project can be open.

You can change the R version that will be installed on the docker image by editing the ``RENKU_BASE_IMAGE`` variable in the ``Dockerfile``:

```
ARG RENKU_BASE_IMAGE=renku/renkulab-r:4.0.0-renku0.10.4-0.6.3
```

A list of available base images can be found at https://hub.docker.com/r/renku/renkulab-r/tags. \
Chose one with the R version you want. For example, for R 3.6.2, you can chose the *3.6.2-renku0.10.4-0.6.3* image.

### R dependencies

This project uses the [renv](https://rstudio.github.io/renv/articles/renv.html) R package to manage R packages dependencies. \
The ``renv.lock`` file is located in the main directory.

Renv has been set to install R packages on the docker image associated with this project instead of within its directory. \
Therefore, the ``renv/`` directory in the main directory of this project has been set as a symbolic link to the renv library located on the docker image, in the ``/home/rstudio`` directory. \
To change this behavior, edit the ``install.R`` file located in the main directory. Don't forget to also change the ``renv`` link into an actual ``renv/`` directory.

The prefered way to install additional R packages and manage them with renv is to use the ``install.packages()``
command within an R session and to update the ``renv.lock`` file using the ``renv::snapshot(type="all")`` command. \
Alternatively, you could also :
1. edit the ``renv.lock`` file - ⚠️ be sure to know what you are doing though. \
OR
2. add the ``install.packages()`` command to the ``install.R`` file. However, this will not update the project's ``renv.lock`` file unless you use ``renv::snapshot()`` within an R session.

⚠️ When using Renku, don't forget to commit and push your changes or they will be lost between sessions. ⚠️

### Python dependencies

The required python packages can be listed in the ``requirements.txt`` file located in the main directory. \
They will be installed on the docker image with ``pip3`` (see the ``Dockerfile`` in the main directory).

### Version control

This project uses Git to track files versions.

## Structure

*Description of the directory tree structure.*

* ``data/``: Contains raw and processed data and metadata, including initial, intermediate data and final data and metadata.
* ``notebooks/``: Contains computational notebooks used to process the data and metadata.
* ``bin/``: Contains executable code.
* ``figs/``: Contains figures.
* ``.renku/``: Contains files used by the Renku platform and its associated ``renku`` python package.

## Content

*Description of the data, metadata and code used in this project. For example:*

* **Data:** *How are data organized and used in this project? What is their source?*
* **Metadata:** *How are metadata organized and used in this project? What is their source?*
* **Code:** *How is the code organized and used in this project?*

## Related studies

*Reference and description to any associated study. For example:*

* **Study title**
    * **Principal investigator:** *Name, contact and affiliation of the principal investigator.*
    * **Associated funding:** *Reference to any funding.*
    * **Associated publications:** *Links to any associated publications.*

## Contributors

*Names, contacts, affiliations and respective contributions of the project authors.*

## Methods

*Description of any method associated to the code or datasets used in the project, any related ontologies and external references.*

## License

*Mention of licenses associated with the project and/or part of the project.*

## Resources

* This project is based on the following template: https://github.com/auwerxlab/renku-project-template.git