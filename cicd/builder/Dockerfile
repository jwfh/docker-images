FROM amazonlinux:latest
LABEL Maintainer="jacob@jwfh.ca"
LABEL Version="0.1.0"
ENV LANG=C.UTF-8
RUN yum update -y && \
	yum install -y \
		acl \
		apr \
		apr-util \
		apr-util-bdb \
		audit-libs \
		autoconf \
		automake \
		avahi-libs \
		bash \
		binutils \
		bison \
		bluez \
		bluez-libs \
		boost-date-time \
		boost-system \
		boost-thread \
		byacc \
		bzip2 \
		bzip2-devel \
		bzip2-libs \
		ca-certificates \
		cmake3 \
		coreutils \
		cpp \
		cracklib \
		cracklib-dicts \
		cryptsetup-libs \
		cscope \
		ctags \
		dbus \
		dbus-libs \
		device-mapper \
		device-mapper-libs \
		diffstat \
		doxygen \
		dwz \
		dyninst \
		efivar-libs \
		elfutils \
		elfutils-default-yama-scope \
		elfutils-libelf-devel \
		elfutils-libs \
		emacs-filesystem \
		expat \
		expat-devel \
		file \
		findutils \
		fipscheck \
		fipscheck-lib \
		flex \
		gcc \
		gcc-c++ \
		gcc-gfortran \
		gdb \
		gdbm \
		gdbm-devel \
		gettext \
		gettext-common-devel \
		gettext-devel \
		gettext-libs \
		git \
		git-core \
		git-core-doc \
		glibc-devel \
		glibc-headers \
		gnutls \
		groff-base \
		gzip \
		hardlink \
		indent \
		intltool \
		json-c \
		jq \
		kernel-devel \
		kernel-headers \
		kmod \
		kmod-libs \
		ksh \
		less \
		libatomic \
		libcap-ng \
		libcilkrts \
		libcroco \
		libdwarf \
		libedit \
		libfdisk \
		libffi \
		libffi-devel \
		libgfortran \
		libgomp \
		libitm \
		libmodman \
		libmpc \
		libmpx \
		libproxy \
		libpwquality \
		libquadmath \
		libsanitizer \
		libsecret \
		libsemanage \
		libsmartcols \
		libtool \
		libutempter \
		libuuid-devel \
		lz4 \
		m4 \
		man \
		make \
		mokutil \
		mpfr \
		ncurses \
		ncurses-devel \
		ncurses-libs \
		neon \
		nettle \
		ninja-build \
		noarch \
		openssh \
		openssh-clients \
		openssl \
		openssl-devel \
		openssl-libs \
		pakchois \
		pam \
		patch \
		patchutils \
		pcre2 \
		perl \
		perl-Carp \
		perl-Data-Dumper \
		perl-Encode \
		perl-Error \
		perl-Exporter \
		perl-File-Path \
		perl-File-Temp \
		perl-Filter \
		perl-Getopt-Long \
		perl-Git \
		perl-HTTP-Tiny \
		perl-PathTools \
		perl-Pod-Escapes \
		perl-Pod-Perldoc \
		perl-Pod-Simple \
		perl-Pod-Usage \
		perl-Scalar-List-Utils \
		perl-Socket \
		perl-Storable \
		perl-TermReadKey \
		perl-Test-Harness \
		perl-Text-ParseWords \
		perl-Thread-Queue \
		perl-Time-HiRes \
		perl-Time-Local \
		perl-XML-Parser \
		perl-constant \
		perl-libs \
		perl-macros \
		perl-parent \
		perl-podlators \
		perl-srpm-macros \
		perl-threads \
		perl-threads-shared \
		pkgconfig \
		qrencode-libs \
		rcs \
		readline \
		readline-devel \
		rpm-build \
		rpm-sign \
		shadow-utils \
		sqlite \
		sqlite-devel \
		subversion \
		subversion-libs \
		swig \
		system-rpm-config \
		systemd \
		systemd-libs \
		systemd-sysv \
		systemtap \
		systemtap-client \
		systemtap-devel \
		systemtap-runtime \
		tar \
		tcsh \
		tk \
		tk-devel \
		trousers \
		unzip \
		ustr \
		util-linux \
		wget \ 
		which \
		xz \
		xz-devel \
		yum-utils \
		zip \
		zlib-devel \
		zsh \
	; \
	yum-config-manager --add-repo "https://download.opensuse.org/repositories/shells:/fish:/release:/3/RHEL_7/shells:fish:release:3.repo"; \
	yum -y install fish
RUN set -ex; \ 
	curl -L -o /tmp/bmake.zip https://github.com/jwfh/bmake/archive/master.zip; \
	mkdir -p /usr/src/bmake; \
	unzip -d /usr/src/bmake /tmp/bmake.zip; \
	rm /tmp/bmake.zip; \
	cd /usr/src/bmake/bmake-master; \
	./boot-strap --prefix=/usr/local --install; \
	cd /; \
	rm -rf /usr/src/bmake
RUN set -ex; \
	export VERSIONS="3.5.9 3.6.10 3.7.7 3.8.2"; \
	for V in $VERSIONS; do \
		cd /; \
		mkdir -p /usr/src/python/$V; \
		curl -L https://www.python.org/ftp/python/$V/Python-$V.tar.xz | tar -x -J -C /usr/src/python/$V --strip-components=1; \
		cd /usr/src/python/$V; \
		./configure \
			--prefix=/usr/local \
			--enable-optimizations \
        ; \
		make -j "$(nproc)"; \
		if [ "$V" == "$(echo $VERSIONS | tr '[:space:]' '\n' | sort -V | tail -n1)" ]; then \
			make install; \
			cd /usr/local/bin; \
			ln -s idle3 idle; \
			ln -s pydoc3 pydoc; \
			ln -s python3 python; \
			ln -s python3-config python-config; \
		else \
			make altinstall; \
		fi; \
		ldconfig; \
		export PYTHON_BINARY="python3.$(echo $V | awk -F. '{ print $2 }')"; \
		if [ "$V" == "$(echo $VERSIONS | tr '[:space:]' '\n' | sort -V | tail -n1)" ]; then \
			if [ "$(python -V)" != "$(python3 -V)" ]; then \
				echo python and python3 versions do not match for primary version $V; \
				exit 1; \
			elif [ "$(python3 -V)" != "$($PYTHON_BINARY -V)" ]; then \
				echo python3 and $PYTHON_BINARY versions do not match for primary version $V; \
				exit 1; \
			fi; \
		else \
			$PYTHON_BINARY -V; \
		fi; \
	done; \
	cd /; \
	rm -rf /usr/src/python
RUN set -ex; \
	export LDFLAGS="-Wl,-rpath /usr/local/lib"; \
	export LD_RUN_PATH=/usr/local/lib; \
	mkdir /usr/src/node; \
	curl -L "$(curl -s https://api.github.com/repos/nodejs/node/releases/latest | jq -r .tarball_url)" | tar -x -z -C /usr/src/node --strip-components=1; \
	cd /usr/src/node; \
	./configure --prefix=/usr/local; \
	make; \
	make install; \
	cd /; \
	rm -rf /usr/src/node
RUN set -ex; \
	npm install -g npm; \
	npm install -g typescript; \
	npm install -g coffeescript; \
	npm install -g eslint; \
	npm install -g gatsby
RUN set -ex; \
	curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y -v
ENV PATH="$HOME/.cargo/bin:$PATH"
RUN set -ex; \
	mkdir -p /usr/src/aws-sdk-cpp; \
	curl -L "https://api.github.com/repos/aws/aws-sdk-cpp/tarball/$(curl -L -s https://api.github.com/repos/aws/aws-sdk-cpp/tags | jq -r .[].name | sort -V | tail -n1)"| tar -x -z -C /usr/src/aws-sdk-cpp --strip-components=1; \
	cd /usr/src/aws-sdk-cpp; \
	mkdir build; \
	cd build; \
	cmake ../ -GNinja -DCMAKE_BUILD_TYPE=Release; \
	ninja; \
	ninja install; \
	cd /; \
	rm -rf /usr/src/aws-sdk-cpp
