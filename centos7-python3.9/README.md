# Python 3.9 for CentOS 7

I need a python 3.9 version running on CentOS 7. This version is not available as RPM on public repositores and
the install process require to make from the source code.

This docker image download the python source (3.9.10 is the latest one) and compile it. Install the 
python on /usr/local/ and made it available using the python3 command.

Also I have installed the pip3 too. 

CentOS 7 has a strong dependency with python 2, you can't rename the default python installer with a python3. 

# How to use it
This image is based on the oficial image created by python team: 

https://github.com/docker-library/python/blob/master/3.9/bullseye/Dockerfile 

With some personal changes (How I install pip3 or the flags used), but the usage should be equals. 
Exammple usage : 
```
docker run ferrosa/centos7-python3.9:latest myfile.py
 
```
