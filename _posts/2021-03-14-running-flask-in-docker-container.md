---
layout: post
title:  'Running Flask in a Docker container'
date:   2021-03-14 00:00:00 -0700
categories: 
---
These are notes from creating and running a Docker container for a simple Flask app. I did the following in more or less chronological order.

Created four images that are layered on each other, as follows. Tested and debugged 
each image as it was created by using  `docker run --interactive --tty` in order to have a 
terminal 
logged into the container running the image. In the terminal I could confirm that software
packages had been installed as intended as well as any other setup. I did this for each image 
except for the last one, which runs the Flask app automatically when a container for the 
image is created.

0) **Ubuntu**. Use Ubuntu 20.04, aka focal, as the base for the first image created here.

1) **Python 3 and Pip 3**. Install python 3.x and pip 3.x.

~~~~~
# Use this dockerfile to build image python3-pip3

FROM ubuntu:focal

RUN apt-get update
RUN apt-get --yes install python3
RUN apt-get --yes install python3-pip
~~~~~

2)  **Flask**. Install Flask 1.1.2, et al, using pip with `requirements.txt` providing the 
list of python packages to install.

~~~~~
# Use this dockerfile to build image flask112

FROM python3-pip3

WORKDIR /espresso
COPY requirements.txt .
RUN pip3 install -r requirements.txt
~~~~~

3)  **Espresso**. Espresso was the name of our app. In this image all of its files are 
copied into the container, environment variables are set, and port 5000 within the container 
is exposed for Flask to listen to. Note that the app is not actually run here. The next image 
does that.

For development purposes it was simpler to do all the setup for the app and not have it 
run here. That way I was able to easily confirm 

~~~~~
# Use this dockerfile to build image espresso

FROM flask112

WORKDIR /espresso
COPY . /espresso

ENV FLASK_APP=espresso.py
ENV FLASK_DEBUG=true
ENV FLASK_ENV=development

EXPOSE 5000
~~~~~

4)  **Run Espresso**. Run the app. At this point all of the setup has been done by the 
underlying images previously built. This image creates user `earl` and runs the container 
as that user. Otherwise, the container by default runs as `root`, which is considered a 
security risk. The dockerfile entrypoint command runs flask upon creation of the container.

Note that the host is specified as 0.0.0.0 which means any host for the originator of 
requests for Flask to handle, as long as the request uses port 5000. Otherwise, the value 
defaults to 127.0.0.1 which is localhost, i.e. the container itself. This does not work 
because requests originate from outside the container.

Specifying any host is not a security 
risk since the container is "enclosed" by a host system and can only be accessed through 
requests forwarded by its host.

Port 5000 is the default for Flask but this flask command makes it explicit.

~~~~~
# Use this dockerfile to build image espresso-run

FROM espresso

WORKDIR /espresso
RUN useradd -u 1000 earl
USER earl

ENTRYPOINT flask run --host 0.0.0.0 --port 5000
~~~~~

<hr /><br />

The remainder of this post is about the options provided to `docker run` for instantiating 
a container that runs the espresso app.

<br />

**Sources:**

<em>Don’t Embed Configuration or Secrets in Docker Images</em>  
<https://medium.com/@mccode/dont-embed-configuration-or-secrets-in-docker-images-7b2e0f916fdd>

<em>Docker Container Security 101: Risks and 33 Best Practices</em>  
<https://www.stackrox.com/post/2019/09/docker-security-101/>

<em>Docker Hardening Guide</em>  
<https://docs.paloaltonetworks.com/cortex/cortex-xsoar/5-5/cortex-xsoar-admin/docker/docker-hardening-guide.html>
