- to create read only volumes add ":ro" after volume syntax
- ex: `docker run -d --rm -p 3000:8080 --name feedback-app -v feedback:/app/feedback -v "/home/lenovo/Desktop/AMIT/udemy/Docker/Docker/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules -v /app/temp feedback-node:volumes`

- to add environment variables add below lines in Dockerfile
- this port 8080 will act as default port number and you can change this port by add `--env`/`-e` tag in command line in docker run command to change port number . for this we don't need to rebuild the image.
  - ENV PORT 8080
  - EXPORT $PORT
- ex: `docker run -d --rm -p 3000:8000 --env PORT=8000 --name feedback-app -v feedback:/app/feedback -v "/home/lenovo/Desktop/AMIT/udemy/Docker/Docker/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules -v /app/temp feedback-node:env`
- to add environment variables from `.env` file add `--env-file ./.env` in docker run command

For Dockerizing Node APP with MongoDB and React SPA:
- in node app :
  - `docker -t goals-node .`
  - `docker run --name goals-backend --add-host=host.docker.internal:172.17.0.1 --rm goals-node` (ok for conneting with db)
  - For ubuntu system : `docker run --name goals-backend --add-host=host.docker.internal:172.17.0.1 --rm -d -p 8080:8080 goals-node` (require this command so as to connect with frontend as it requires port number also)
  - For other Systems: `docker run --name goals-backend --rm goals-node`
- to run mongoDB container:
  - `docker container prune` (Optional)
  - `docker run --name mongodb --rm -d -p 27017:27017 mongo`
- to run frontend react SPA:
  - Add urls for fetch requests as `http://localhost:8080/goals`
