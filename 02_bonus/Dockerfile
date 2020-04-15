FROM debian:10

RUN apt update && \
	apt install -y tar \
					git \
					wget \
					curl \
					make \
					python2.7 \
					python-pip \
					python3.7 \
					python3-pip \
					cowsay \
					golang \
					zsh && \
					echo "y" | sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

RUN apt install -y libcurl4-openssl-dev libssl-dev

RUN pip3 install	ipython \
					requests \
					pwntools
RUN pip install		html

RUN echo -e " \
╔═══╗╔══╗╔═══╗╔══╗╔╗──╔╗╔══╗ \n\
║╔═╗║║╔╗║║╔═╗║║╔╗║║║──║║║╔═╝ \n\
║╚═╝║║╚╝║║╚═╝║║╚╝║║╚╗╔╝║║╚═╗ \n\
║╔══╝║╔╗║║╔╗╔╝║╔╗║║╔╗╔╗║╚═╗║ \n\
║║───║║║║║║║║─║║║║║║╚╝║║╔═╝║ \n\
╚╝───╚╝╚╝╚╝╚╝─╚╝╚╝╚╝──╚╝╚══╝ \n\
"
WORKDIR /opt/detect_params
RUN git clone https://github.com/s0md3v/Arjun.git arjun && \
	chmod +x /opt/detect_params/arjun/arjun.py && \
	ln -s /opt/detect_params/arjun/arjun.py /usr/bin/arjun

# ╔══╗╔═══╗╔══╗
# ║╔╗║║╔═╗║╚╗╔╝
# ║╚╝║║╚═╝║─║║
# ║╔╗║║╔══╝─║║
# ║║║║║║───╔╝╚╗
# ╚╝╚╝╚╝───╚══╝
WORKDIR /opt/find_api_links
RUN git clone https://github.com/GerbenJavado/LinkFinder && \
	cd LinkFinder && \
	pip3 install -r requirements.txt && \
	python setup.py install && \
	chmod +x linkfinder.py && \
	ln -s /opt/find_api_links/LinkFinder/linkfinder.py /usr/bin/linkfinder

EXPOSE 8008
WORKDIR /opt/find_api_links
RUN git clone https://github.com/nahamsec/JSParser.git && \
	cd JSParser && \
	pip install -r requirements.txt && \
	python setup.py install && \
	chmod +x handler.py && \
	sed -i '1s/^/#!\/usr\/bin\/env python\n/' handler.py && \
	ln -s /opt/find_api_links/JSParser/handler.py /usr/bin/jsparser


# ╔══╗╔═══╗╔╗
# ║╔═╝║╔═╗║║║
# ║╚═╗║║─║║║║
# ╚═╗║║║╔╝║║║
# ╔═╝║║╚╝─║║╚═╗
# ╚══╝╚═══╝╚══╝
WORKDIR /opt/sql
RUN git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev && \
	ln -s /opt/sql/sqlmap-dev/sqlmap.py /usr/bin/sqlmap

RUN git clone https://github.com/codingo/NoSQLMap.git nosqlmap && \
	cd nosqlmap && \
	python setup.py install && \
	ln -s /opt/sql/nosqlmap/nosqlmap.py /usr/bin/nosqlmap
	

# ╔══╗─╔══╗╔═══╗╔══╗
# ║╔╗╚╗╚╗╔╝║╔═╗║║╔═╝
# ║║╚╗║─║║─║╚═╝║║╚═╗
# ║║─║║─║║─║╔╗╔╝╚═╗║
# ║╚═╝║╔╝╚╗║║║║─╔═╝║
# ╚═══╝╚══╝╚╝╚╝─╚══╝

WORKDIR /root/
RUN go get github.com/OJ/gobuster
WORKDIR /root/go/src/github.com/OJ/gobuster
RUN go build && \
	make linux && \
	ln -s /root/go/src/github.com/OJ/gobuster/build/gobuster-linux-amd64/gobuster /usr/bin/gobuster

WORKDIR /opt/
RUN git clone https://github.com/danielmiessler/SecLists wordlists

RUN /usr/games/cowsay "done!"

ENTRYPOINT zsh
