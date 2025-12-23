FROM mcr.microsoft.com/devcontainers/javascript-node:20 as NODE

FROM mcr.microsoft.com/devcontainers/java:17

# Copy Node.js sys folders
COPY --from=NODE /usr/local /usr/local

# Android SDK setup
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_HOME=/opt/android-sdk
ENV PATH=${PATH}:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools

RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools

ARG CMDLINE_TOOLS_ZIP=commandlinetools-linux-11076708_latest.zip
ARG CMDLINE_TOOLS_URL=https://dl.google.com/android/repository/${CMDLINE_TOOLS_ZIP}

RUN curl -fsSL ${CMDLINE_TOOLS_URL} -o /tmp/${CMDLINE_TOOLS_ZIP} \
    && unzip /tmp/${CMDLINE_TOOLS_ZIP} -d ${ANDROID_SDK_ROOT}/cmdline-tools \
    && mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest \
    && rm /tmp/${CMDLINE_TOOLS_ZIP}

RUN yes | sdkmanager --licenses

WORKDIR /workspace

CMD "bash"
