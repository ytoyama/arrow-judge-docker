FROM docker.io/ubuntu:14.04

RUN locale-gen

RUN apt-get update --fix-missing
RUN apt-get -y install build-essential software-properties-common vim

RUN apt-add-repository ppa:hiromu1996/arrow-judge && apt-get update
RUN apt-get -y install arrow-judge

ADD server.conf /etc/arrow-judge/server.conf
ADD __init__.py.diff /tmp/__init__.py.diff
RUN patch /usr/share/pyshared/arrow_judge/__init__.py < /tmp/__init__.py.diff
RUN rm /usr/lib/python2.7/dist-packages/arrow_judge/*.pyc
RUN sed 's/\(\/\)\(cpuacct\(\/\|$\)\)/\1cpu,\2/g' \
        /usr/lib/python2.7/dist-packages/arrow_judge/sandbox.py > tmp.py && \
    mv tmp.py /usr/lib/python2.7/dist-packages/arrow_judge/sandbox.py

EXPOSE 0-65535
CMD hostname 127.0.0.1 && \
    arrow-judge start && \
    tailf /var/log/arrow_judge/error.log
