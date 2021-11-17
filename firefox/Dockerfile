FROM ubuntu:latest                                                                 
RUN apt-get update && apt-get install -y firefox
RUN useradd --shell /bin/bash --create-home developer && \
    usermod -aG sudo developer
  
USER developer
ENV HOME /home/developer
CMD /usr/bin/firefox