FROM ubuntu
MAINTAINER Ronan Klyne <dockers@rklyne.net>

RUN apt-get update -y &&\
    apt-get install -y python-pip &&\
    apt-get clean

CMD python -c "from wsgiref import simple_server; simple_server.make_server('0.0.0.0', 80, simple_server.demo_app).serve_forever()"

EXPOSE 80


