FROM ubuntu:latest

## PART 1: Core components
## =======================

# Install utilities
RUN apt-get update --fix-missing && apt-get -y upgrade &&\
apt-get install -y sudo curl wget unzip git

# Install node 6
RUN curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - &&\
sudo apt-get install -y nodejs

# Install Xvfb and dbus for X11
# RUN apt-get install -y xvfb dbus-x11

# Install Chrome for Ubuntu
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add - &&\
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list' &&\
sudo apt-get update &&\
sudo apt-get install -y google-chrome-stable

# Install imagemagick for tests [optional]
# RUN apt-get install -y imagemagick

# Copy key documents (except .dockerignored files)
# COPY etc/xvfb /etc/init.d/xvfb
# RUN chmod +x /etc/init.d/xvfb

# Add a user and make it a sudo user
RUN useradd -m chromeuser &&\
sudo adduser chromeuser sudo

# Copy the chrome-user script used to start Chrome as non-root
COPY chromeuser-script.sh /
RUN chmod +x /chromeuser-script.sh

## PART 2: Lighthouse
## ==================

# Download lighthouse
RUN git clone https://github.com/googlechrome/lighthouse &&\
cd /lighthouse &&\
git checkout tags/1.6.0 &&\
npm install &&\
npm run install-all &&\
npm run build-all

## PART 3: Express server
## ======================

# Install express
COPY package.json /
RUN npm install

# Add the simple server file
COPY server.js /
RUN chmod +x /server.js

# Expose port 8080
EXPOSE 8080

## PART 4: Final setup
## ===================

# Set the entrypoint
COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# CMD ["/bin/bash"]