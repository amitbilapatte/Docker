# **COMMANDS**

- To create read only volumes add ":ro" after volume syntax
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

- **TO Add DOCKER NETWORK:**

- Add mongo in network =>
  - `docker network create goals-net` //goals-net is network name here
  - `docker run --name mongodb --rm -d --network goals-net mongo`
- Add backend in network =>
  - change code in App.js for mongoose.connect method like this: `mongoose.connect("mongodb://mongodb:27017/course-goals",...remaining code ...`
  - rebuild image `docker build-t goals-node .`
  - run `docker run --name goals-backend --rm -d --network goals-net goals-node` (it won't work for frontend as frontend can't access backend as react frontend works on browser, and backend is on server and we haven't provided any port to accesss it)
  - run `docker run --name goals-backend --rm -d -p 8080:8080 --network goals-net goals-node` (now frontend can access it via port number)

- Add frontend
  - `docker build -t goals-react .`
  - `docker run --name goals-frontend --network goals-net --rm -p 3000:3000 -it goals-react`

- **FOR DATA PERSISTENCE**
  - `docker stop mongodb`
  - `docker run --name mongodb --rm -d --network goals-net mongo` // now we can't see previously added goals as data is lost when mongodb container was stopped
  - to prevent data loss use volume when creating the container: `docker run --name mongodb -v data:/data/db --rm -d --network goals-net mongo`

- **FOR MONGODB USER AUTHENTICATION**
  - `docker stop mongodb`
  - `docker run --name mongodb -v data:/data/db --rm -d --network goals-net -e MONGO_INTIDB_ROOT_USERNAME=amit -e MONGO_INTIDB_ROOT_USERNAME=amitpassword mongo`
  - Now in backend app change in mongoose.connect method `mongoose.connect("mongodb://amit:amitpassword@mongodb:27017/course-goals?authSource=admin",...remaining code ...`

- `docker run node`
- `docker run -it -d node`
- `docker exec -it containerName npm init`
- `docker run -it node npm init`
- `docker build -t node-util .`
- `docker run -it -v /user/AMIT/Docker/appname:/app node-util npm init`
- `docker build -t mynpm .`
- `docker run -it -v /user/AMIT/Docker/appname:/app node-util mynpm install`
- `docker-compose up`
<!-- above command will autoexit -->
- `docker-compose --rm run npm init`

- **FOR LARAVEL APPLICATION**

- after adding `docker-compose.yaml` file, we have to target on single service `composer` to install the laravel basic setup by `docker-compose run` command as follows:
 `docker-compose run --rm composer create-project --prefer-dist laravel/laravel .`

- This project didn't work on my end (may be ubuntu issue).
