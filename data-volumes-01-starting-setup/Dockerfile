FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

ENV PORT=8080

EXPOSE $PORT

# VOLUME [ "/app/node_modules" ]
# ===========
# either use above line in docker file or use "-v /app/node_modules" in below code, 
# if we use both at the same time, then it will run in infinite loop on localhost, and it will give logs like this: "UnhandledPromiseRejectionWarning: Error: EXDEV: cross-device link not permitted, rename '/app/temp/hey.txt' -> '/app/feedback/hey.txt'"
# ===========
# docker run -d -p 3000:8080 --name feedback-app -v feedback:/app/feedback -v  "/home/lenovo/Desktop/AMIT/udemy/Docker/data-volumes-01-starting-setup:/app" -v /app/node_modules feedback-node:volumes

CMD [ "npm", "start" ]