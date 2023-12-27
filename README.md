- to create read only volumes add ":ro" after volume syntax
- ex: `docker run -d --rm -p 3000:8080 --name feedback-app -v feedback:/app/feedback -v "/home/lenovo/Desktop/AMIT/udemy/Docker/Docker/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules -v /app/temp feedback-node:volumes`

- to add environment variables add below lines in Dockerfile
- this port 8080 will act as default port number and you can change this port by add `--env`/`-e` tag in command line in docker run command to change port number . for this we don't need to rebuild the image.
  - ENV PORT 8080
  - EXPORT $PORT
- ex: `docker run -d --rm -p 3000:8000 --env PORT=8000 --name feedback-app -v feedback:/app/feedback -v "/home/lenovo/Desktop/AMIT/udemy/Docker/Docker/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules -v /app/temp feedback-node:env`
- to add environment variables from `.env` file add `--env-file ./.env` in docker run command
