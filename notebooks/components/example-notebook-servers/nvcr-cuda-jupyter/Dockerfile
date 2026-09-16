#
# NOTE: Use the Makefiles to build this image correctly.
#

ARG BASE_IMG=nvcr-cuda-jupyter:1.0
FROM $BASE_IMG

ARG TARGETARCH

USER root


# create the SYSTEM_CONFIG_PATH for jupyter, and make it owned by NB_USER
# this is needed for jupyter to write `--level=system` configs
#RUN mkdir -pv /usr/local/etc/jupyter \
RUN chown -R ${NB_USER}:${NB_GID} /usr/local/etc/jupyter

# switch to NB_UID for installs
USER $NB_UID

# install - requirements.txt
COPY --chown=${NB_USER}:${NB_GID} requirements.txt /tmp
RUN python3 -m pip install -r /tmp/requirements.txt --quiet --no-cache-dir \
 && rm -f /tmp/requirements.txt

# s6 - copy scripts
COPY --chown=${NB_USER}:${NB_GID} --chmod=755 s6/ /etc

# configure - jupyter
# NOTE: we use `--level=system` to write these configs at `/usr/local/etc/jupyter` because it defaults
##      to `/opt/conda/etc/jupyter` and conda cleans that directory after every install
RUN jupyter labextension disable --level=system "@jupyterlab/apputils-extension:announcements" \
 && jupyter labextension lock --level=system "@jupyterlab/apputils-extension"

# s6 - 01-copy-tmp-home
# NOTE: the contents of $HOME_TMP are copied to $HOME at runtime
#       this is a workaround because a PVC will be mounted at $HOME
#       and the contents of $HOME will be hidden
RUN cp -p -r -T "${HOME}" "${HOME_TMP}" \
    # give group same access as user (needed for OpenShift)
 && chmod -R g=u "${HOME_TMP}"

EXPOSE 8888
