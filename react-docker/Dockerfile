FROM node:20-alpine

#create a user with permission to run the app
# -S -> create a system user
# -G -> add the user to a group
# this is done to avoid running the app as root
# if the app is run as root , any vulnerability in the app can be exploited to gain to tht host sytem
# it is good practise to run the app as a non root user
RUN addgroup app && adduser -S -G app app

#set the user to run the app
USER app

#set the working directoru to /app
WORKDIR /app

#copy package.json and package-lock.json to the working directories
# this is done before copying the rest od the diles to take advantage of docker's cache
# if the package.json and package-lock.json files have not changed , docker will use the cached dependencies
COPY  package*.json ./

#sometimes the owenership of the files in the working directories is changed to root
# and thus the app can not access the files and throws an errro -> EACCESS: permission denied
# to avoid this , chnage the ownership of the files to the root user
USER root

#chown command changes the user and/or grou[ ownershi[ of for given files
RUN chown -R app:app .

#chnage the user back to the app user
USER app

#install dependencies
RUN npm install

#copy rest of the files to the working directory
COPY . .


#expose the prort 5173 to tell docker that the constainer listen on the specified network port at runtime
EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]
