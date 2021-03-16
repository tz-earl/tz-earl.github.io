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
run here. That way I was able to easily confirm all the files were in place, the environment 
variables set up, and the desired port exposed.

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
security risk. The dockerfile entrypoint command invokes flask upon creation of the container.

Note that the host for the url request is specified as 0.0.0.0 which means any host for  Flask to respond to as long as the request uses port 5000. Otherwise, the host value 
defaults to 127.0.0.1 which is localhost, i.e. the container itself. This does not work 
because requests originate from outside the container.

Specifying any url host is not a security risk since the container is enclosed by a real 
(host) system and can only be accessed through requests forwarded by it.

Port 5000 is the default for Flask but this invocation of flask makes the port number explicit.

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
a container that runs the espresso app in the last image described above.

Each option is described separately, then they are all put together at the end of this post.

**--mount**

Mount the `.env` file on the host system (source) that contains application secrets to a 
corresponding `.env` file inside the container (target). `readonly` is a good security 
practice.

~~~~~
--mount type=bind,source=/home/earl/PycharmProjects/espresso/.env,target=/espresso/.env,readonly
~~~~~

**-p**

Map port 5055 from the host system to port 5000 of the container. I chose port 5055 
arbitrarily. Port 5000 matches the port that was exposed in one of the images above as well 
as the port number specified for the invocation of Flask in the final image.

~~~~~
-p 5055:5000
~~~~~

**--cap-drop**

This is a security practice that disables the use of the `setgid()` and `setuid()` functions 
inside the container. It prevents any malicious code from running as a different user or a 
different group.

~~~~~
 --cap-drop SETGID --cap-drop SETUID 
~~~~~

**--pids-limit**

~~~~~
--pids-limit 256
~~~~~

Sets a maximum number processes that can run in the container. Prevents malicious code from 
forking off an unlimited number of processes, aka "fork bomb". I am just guessing that 256 is 
a large enough value.


**--memory**

On my Ubuntu laptop, this limits real memory but not swap memory so it's of limited effect. 
However, on a production server the situation may be different and this constraint is 
hopefully truly effective in that swap memory is also limited.

~~~~~
--memory 1G
~~~~~


<br />

**Sources:**

<em>Don’t Embed Configuration or Secrets in Docker Images</em>  
<https://medium.com/@mccode/dont-embed-configuration-or-secrets-in-docker-images-7b2e0f916fdd>

<em>Docker Container Security 101: Risks and 33 Best Practices</em>  
<https://www.stackrox.com/post/2019/09/docker-security-101/>

<em>Docker Hardening Guide</em>  
<https://docs.paloaltonetworks.com/cortex/cortex-xsoar/5-5/cortex-xsoar-admin/docker/docker-hardening-guide.html>
