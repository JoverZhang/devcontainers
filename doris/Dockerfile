FROM ubuntu:24.04

# install dependencies
RUN apt-get update && apt-get install -y \
    byacc patch automake libtool linux-tools-generic vim make which file libncurses-dev gettext unzip bzip2 zip util-linux \
    wget git python-is-python3 bison openjdk-8-jdk \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# install maven 3.6.3
RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
    && wget -q -O /tmp/apache-maven.tar.gz https://doris-thirdparty-1308700295.cos.ap-beijing.myqcloud.com/tools/apache-maven-3.6.3-bin.tar.gz \
    && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
    && rm -f /tmp/apache-maven.tar.gz \
    && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

# install nodejs
RUN wget https://doris-thirdparty-1308700295.cos.ap-beijing.myqcloud.com/tools/node-v12.13.0-linux-x64.tar.gz \
    -q -O /tmp/node-v12.13.0-linux-x64.tar.gz \
    && cd /tmp/ && tar -xf node-v12.13.0-linux-x64.tar.gz \
    && cp -r node-v12.13.0-linux-x64/* /usr/local/ \
    && rm /tmp/node-v12.13.0-linux-x64.tar.gz && rm -rf node-v12.13.0-linux-x64

# install jdk17
RUN wget https://doris-thirdparty-1308700295.cos.ap-beijing.myqcloud.com/tools/openjdk-17.0.2_linux-x64_bin.tar.gz \
    -q -O /tmp/openjdk-17.0.2_linux-x64_bin.tar.gz \
    && cd /tmp && tar -xzf openjdk-17.0.2_linux-x64_bin.tar.gz \
    && cp -r jdk-17.0.2/ /usr/lib/jvm/ \
    && rm /tmp/openjdk-17.0.2_linux-x64_bin.tar.gz && rm -rf /tmp/jdk-17.0.2/

# install ldb-toolchain
# 0.17
RUN wget https://doris-community-bj-1308700295.cos.ap-beijing.myqcloud.com/tools/ldb_toolchain_gen.sh \
    -q -O /tmp/ldb_toolchain_gen.sh \
    && sh /tmp/ldb_toolchain_gen.sh /var/local/ldb-toolchain/ \
    && rm /tmp/ldb_toolchain_gen.sh

# there is a repo which is included all of thirdparty
ENV REPOSITORY_URL="https://doris-thirdparty-hk-1308700295.cos.ap-hongkong.myqcloud.com/thirdparty" \
    DEFAULT_DIR="/var/local" \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0" \
    PATH="/var/local/ldb-toolchain/bin/:$PATH"

# install ccache
RUN wget "https://doris-community-bj-1308700295.cos.ap-beijing.myqcloud.com/tools/ccache-4.8.tar.gz" \
    -q -O /tmp/ccache-4.8.tar.gz \
    && cd /tmp/ && tar xzf ccache-4.8.tar.gz \
    && cd ccache-4.8 \
    && cmake -B _build -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_BUILD_TYPE=Release . \
    && cmake --build _build --config Release -j 4 \
    && cp _build/ccache /var/local/ldb-toolchain/bin/
RUN rm -f /etc/profile.d/ccache.*

ENV JAVA_HOME="/usr/lib/jvm/jdk-17.0.2" \
    MAVEN_HOME="/usr/share/maven" \
    REPOSITORY_URL="https://doris-thirdparty-repo.bj.bcebos.com/thirdparty" \
    DEFAULT_DIR="/var/local"
ENV PATH="/var/local/ldb-toolchain/bin/:/var/local/thirdparty/installed/bin/:$JAVA_HOME/bin/:$PATH"

USER 1000
WORKDIR /home/workspace

CMD ["/bin/bash"]

