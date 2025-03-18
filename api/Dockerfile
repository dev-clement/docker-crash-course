FROM node:16-alpine

# note: Each line represents a layer, the FROM is the base image this image
# is coming from.

RUN npm install -g nodemon

# Installing nodemon to the container without doing any installations
# nodemon is a node package that is used for restarting the server
# when a change on the code occurs, this way using the volumes the 
# modifications happening in the host will be directly added to the
# container without rebuilding the image

WORKDIR /app

# WORKDIR command is used to specify the working directory of the image
# that means when the code COPY . . will be ran, the code present in
# this folder will be placed where the WORKDIR is. on its own folder
# created for it.

COPY package.json .

# Before running the npm install command, it might be a good idea to
# copy the package.json file in order for the npm install to work
# and avoiding some layer to be rebuild on top of the npm install
# command like copying the entire source directory

RUN npm install

# RUN will execute a command passed as parameter, the command here is 
# simply "npm install" to install all the needed dependencies from the
# package.json

COPY . .

# Copy the content of this folder into a /app folder present in the container
# as the image (and the container) has their own folder structure, the content
# of the dot folder isn't present in the container, after the COPY command,
# it will be present in the app folder.

EXPOSE 4000

# Instruction EXPOSE is done to specify which ports the container should
# expose to the outside world. Looking at the app.js file, server is
# listening to the port 4000, meaning that we need to expose it for the
# outside world to send request into it.

CMD ["npm", "run", "dev"]

# Command ran once the container is ready, and when everything
# is set up for the application in the container. The CMD
# will spin up the application inside this container