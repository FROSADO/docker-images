FROM centos:7 AS base

RUN yum -y update && \
    yum -y install wget gcc openssl-devel bzip2-devel libffi-devel zlib-devel make && \
    yum clean all && \
    rm -rf /var/cache/yum

ENV PYTHON_VERSION 3.9.10
ENV GPG_KEY E3FF2839C048B25C084DEBE9B26995E310250568

FROM base as compile
# Download the code and compile
RUN set -eux; \
    wget -O python.tgz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tgz" ;\
    wget -O python.tgz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tgz.asc"; \
    GNUPGHOME="$(mktemp -d)"; export GNUPGHOME; \
	gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys "$GPG_KEY"; \
	gpg --batch --verify python.tgz.asc python.tgz; \
	command -v gpgconf > /dev/null && gpgconf --kill all || :; \
	rm -rf "$GNUPGHOME" python.tgz.asc; \
    mkdir -p /usr/src/python ;\
    tar --extract --directory /usr/src/python --strip-components=1 --file python.tgz ;\
    rm python.tgz;\
    cd /usr/src/python ;\
    /usr/src/python/configure \
          --enable-loadable-sqlite-extensions \
		  --enable-optimizations \
		  --enable-option-checking=fatal \
          ;\
    make altinstall
FROM compile as install

RUN set -eux; \
	bin="$(readlink -ve /usr/local/bin/python3.9)"; \
	dir="$(dirname "$bin")"; \
	cd /; \
    ldconfig;\
    cd /; \
	rm -rf /usr/src/python; \
    find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests -o -name idle_test \) \) \
			-o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name '*.a' \) \) \
			-o \( -type f -a -name 'wininst-*.exe' \) \
		\) -exec rm -rf '{}' + \
	; \
    ln -svT /usr/local/bin/python3.9 /usr/local/bin/python3 ;\
    ln -svT /usr/local/bin/idle3.9 /usr/local/bin/idle3 ;\
    ln -svT /usr/local/bin/python3.9-config /usr/local/bin/python3-config ;\
    ln -svT /usr/local/bin/pydoc3.9 /usr/local/bin/pydoc3 ;\
    python3 -m pip install -q pip --upgrade ;\
    python3 --version ;\
    pip3 install -q pip --upgrade && \
    pip3 install -q wheel virtualenv && \
    pip3 cache purge && \
    pip3 --version
    

CMD ["python3"]