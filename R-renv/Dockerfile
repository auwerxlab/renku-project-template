# For finding latest versions of the base image see
# https://github.com/SwissDataScienceCenter/renkulab-docker

{% if Rversion %}
ARG RVERSION={{ Rversion }}
{% else %}
ARG RVERSION=4.0.0
{% endif %}
ARG RENKU_BASE_IMAGE=renku/renkulab-r:${RVERSION}-0.7.4
FROM ${RENKU_BASE_IMAGE}

# Uncomment and adapt if code is to be included in the image
# COPY src /code/src

# Install system requirements
USER root
RUN apt-get -y update && \
  apt-get clean && \
  apt-get install -y --no-install-recommends \
  apt-utils \
    libncurses5-dev \
    libncursesw5-dev \
    parallel \
    libgit2-dev \
    tk-dev

# Add user to the sudoers
RUN adduser ${NB_USER} sudo && \
    echo "${NB_USER} ALL=(ALL:ALL) ALL" >> /etc/sudoers
USER ${NB_USER}

# install the R dependencies
COPY install.R /tmp/
COPY renv.lock /home/rstudio/renv.lock
RUN R -f /tmp/install.R

## Clean up the /home/rstudio directory to avoid confusion in nested R projects
RUN rm /home/rstudio/.Rprofile; rm /home/rstudio/renv.lock

## Create a symbolic link to the renv directory at startup
USER root
RUN echo "ln -s /home/rstudio/renv /home/rstudio/{{ __sanitized_project_name__ }}/renv" >> /post-init.sh
USER ${NB_USER}

# install the python dependencies
COPY requirements.txt /tmp/
RUN pip3 install -r /tmp/requirements.txt

# RENKU_VERSION determines the version of the renku CLI
# that will be used in this image. To find the latest version,
# visit https://pypi.org/project/renku/#history.
ARG RENKU_VERSION=0.12.1

########################################################
# Do not edit this section and do not add anything below

RUN if [ -n "$RENKU_VERSION" ] ; then \
    pipx uninstall renku && \
    pipx install --force renku==${RENKU_VERSION} \
    ; fi

########################################################
