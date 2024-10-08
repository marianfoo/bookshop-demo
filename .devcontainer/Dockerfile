#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

ARG VARIANT="20-bookworm"
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:${VARIANT}

# The javascript-node image includes a non-root node user with sudo access. Use 
# the "remoteUser" property in devcontainer.json to use it. On Linux, the container 
# user's GID/UIDs will be updated to match your local UID/GID when using the image
# or dockerFile property. Update USER_UID/USER_GID below if you are using the
# dockerComposeFile property or want the image itself to start with different ID
# values. See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=node
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Alter node user as needed, install tslint, typescript. eslint is installed by javascript image
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
    groupmod --gid $USER_GID $USERNAME \
    && usermod --uid $USER_UID --gid $USER_GID $USERNAME \
    && chown -R $USER_UID:$USER_GID /home/$USERNAME; \
    fi \
    #
    # Install tslint, typescript. eslint is installed by javascript image
    && sudo -u ${USERNAME} npm install -g typescript \
    && sudo -u ${USERNAME} npm install -g @sap/cds-dk \
    && sudo -u ${USERNAME} npm install -g @ui5/cli
#    && curl -H "cookie: eula_3_1_agreed=tools.hana.ondemand.com/developer-license-3_1.txt" -o /tmp/cds.zip https://tools.hana.ondemand.com/additional/vscode-cds-updateSite/vscode-cds-2.2.0.vsix \
#    && mkdir -p /tmp/cds \
#    && unzip /tmp/cds.zip extension/*.* -d /tmp/cds \
#    && mkdir -p /root/.vscode-server/extensions/sapse.vscode-cds-2.2.0 \
#    && mv /tmp/cds/extension/* /root/.vscode-server/extensions/sapse.vscode-cds-2.2.0/ 

# *********************************************************************
# * Uncomment this section to use RUN to install other dependencies.  *
# * See https://aka.ms/vscode-remote/containers/dockerfile-run        *
# *********************************************************************
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get -y install sqlite3 \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
ENV DEBIAN_FRONTEND=dialog

# Uncomment to default to non-root user
# USER $USER_UID
