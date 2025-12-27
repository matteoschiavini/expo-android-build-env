# Retrieve Node.js, JDK and Android Build Tools versions
ARG NODE_VERSION
ARG JDK_VERSION
ARG BUILD_TOOLS_VERSION

FROM mcr.microsoft.com/devcontainers/javascript-node:${NODE_VERSION} as NODE

FROM mcr.microsoft.com/devcontainers/java:${JDK_VERSION}

# Copy Node.js sys folders
COPY --from=NODE /usr/local /usr/local

# Android SDK environment
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_HOME=/opt/android-sdk
ENV PATH=${PATH}:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools

RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools

# Gradle & Expo environment
ENV GRADLE_USER_HOME=/gradle
ENV CI=true

RUN mkdir -p ${GRADLE_USER_HOME}

ARG CMDLINE_TOOLS_ZIP=commandlinetools-linux-11076708_latest.zip
ARG CMDLINE_TOOLS_URL=https://dl.google.com/android/repository/${CMDLINE_TOOLS_ZIP}

RUN curl -fsSL ${CMDLINE_TOOLS_URL} -o /tmp/${CMDLINE_TOOLS_ZIP} \
    && unzip /tmp/${CMDLINE_TOOLS_ZIP} -d ${ANDROID_SDK_ROOT}/cmdline-tools \
    && mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest \
    && rm /tmp/${CMDLINE_TOOLS_ZIP}

RUN yes | sdkmanager --licenses

# Install Android SDK components required by Expo
RUN sdkmanager \
    "platform-tools" \ 
    "platforms;android-${BUILD_TOOLS_VERSION}" \
    "build-tools;${BUILD_TOOLS_VERSION}.0.0"

# Make both Android SDK root and Gradle home writable
RUN chown -R $USER:$USER ${ANDROID_SDK_ROOT} ${GRADLE_USER_HOME}
