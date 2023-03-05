FROM mcr.microsoft.com/devcontainers/miniconda:0-3
RUN conda install -n base -c conda-forge mamba
COPY environment.yml* .devcontainer/noop.txt /tmp/conda-tmp/
RUN if [ -f "/tmp/conda-tmp/environment.yml" ]; then umask 0002 && /opt/conda/bin/mamba env update -n base -f /tmp/conda-tmp/environment.yml; fi \
    && rm -rf /tmp/conda-tmp
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends qt6-base-dev

# enable x11 forwarding based on
# https://github.com/microsoft/vscode-dev-containers/blob/main/repository-containers/images/github.com/microsoft/vscode/.devcontainer/base.Dockerfile
COPY .devcontainer/library-scripts/desktop-lite-debian.sh /tmp/library-scripts/
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
	&& bash /tmp/library-scripts/desktop-lite-debian.sh \
	&& apt-get -y install firefox-esr \
	&& echo 'export DBUS_SESSION_BUS_ADDRESS=$(grep -z DBUS_SESSION_BUS_ADDRESS /proc/$(pidof fluxbox)/environ|cut -d= -f2-)' | tee -a /etc/bash.bashrc > /etc/zsh/zshenv \
	&& apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts/

# Core environment variables for X11, VNC, and fluxbox
ENV DBUS_SESSION_BUS_ADDRESS="autolaunch:" \
	VNC_RESOLUTION="1440x768x16" \
	VNC_DPI="96" \
	VNC_PORT="5901" \
	NOVNC_PORT="6080" \
	DISPLAY=":1" \
	LANG="en_US.UTF-8" \
	LANGUAGE="en_US.UTF-8"


# for debian bullseye
RUN echo 'deb http://deb.debian.org/debian bullseye-backports main' | tee /etc/apt/sources.list.d/bullseye-backports.list \
    && apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends -t bullseye-backports qt6-base-dev

ENTRYPOINT ["/usr/local/share/desktop-init.sh"]
CMD ["sleep", "infinity"]


# COPY --chown=$MAMBA_USER:$MAMBA_USER environment.yml /tmp/environment.yml
# RUN micromamba install -y -n base -f /tmp/environment.yml && \
#     micromamba clean --all --yes

# FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}
# Copy environment.yml (if found) to a temp location so we update the environment. Also
# copy "noop.txt" so the COPY instruction does not fail if no environment.yml exists.

# # Install miniconda
# ENV CONDA_DIR /opt/conda
# RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
#      /bin/bash ~/miniconda.sh -b -p /opt/conda

# # Put conda in path so we can use conda activate
# ENV PATH=$CONDA_DIR/bin:$PATH

# COPY environment.yml* .devcontainer/noop.txt /tmp/conda-tmp/
# RUN if [ -f "/tmp/conda-tmp/environment.yml" ]; then umask 0002 && /opt/conda/bin/conda env update -n base -f /tmp/conda-tmp/environment.yml; fi \
#     && rm -rf /tmp/conda-tmp

# # [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends qt6-base-dev
