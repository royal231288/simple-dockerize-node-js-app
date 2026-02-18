### Containerization of a simple react app

##### Step:1 Make a docker file into the root folder of the project
```bash
touch Dockerfile
```
##### Step:2 Open the file and copy and patse given code
```bash
FROM node:18-alpine

WORKDIR /app 

COPY package*.json ./

RUN npm install --production

COPY . . 

EXPOSE 5000

ENV PORT=5000

CMD ["npm", "start"]
```
##### Step:3 Build the docker image
  - Run the following command in the root folder
  ```bash
  docker build -t node-app:1.0 . # here node-app is image name and 1.0 is tag and dot is indicates location of the docker file
  ```
  - Test the images 
  ```bash
  docker images 
  ```
#### Step:3 Now run the image into container
   ```bash
   docker run -p 5001:5000 --name express_app node-app:1.0 # express_app is container name and node-app:1.0 is the image , 5001 is the host machine port and 5000 is container port app to be running
   ```
#### Step:4 Test the app is running or not
  - Open the browser and visit the link
    ```bash
        http://localhost:5001
    ```
#### Step:5 Push the images into the docker hub
  - First login into the docker hub
    ```bash
    docker login
    ```
  - Run the given command
    ```bash
    docker tag node-app:1.0 royal794/node-app:1.0 # here royal794 is usernane of docker hub
    docker push royal794/node-app:1.0
    ```
#### Step:6 Create docker-compose.yml file 
  - Create docker-compose.yml
    ```bash
    touch docker-compose.yml
    ```
  - Open and patse given below code 
  ```bash
    version: "3.8"

    services:

    app:
        image: royal794/node-app:1.0
        container_name: express_app
        expose:
        - "5000"

    nginx:
        image: nginx:alpine
        container_name: nginx_server
        ports:
        - "8080:80"
        depends_on:
        - app
        volumes:
        - ./nginx.conf:/etc/nginx/conf.d/default.conf

  ```
#### Step:7 Run the containers used in the compose file
  - Run the below command into the root directory
  ```bash
  docker compose up -d
  ```

### Step:8 Test the application 
  - Visit the given link
  ```bash
  http://localhost:8080
  ```

#### Some important docker command given below 
  -  To check images 
  ```bash
  docker images
  ```
  - To check containers
  ```bash
  docker ps -a
  ```
  - To stop containr 
  ```bash
    docker stop 12c3bfc83c8f # 12c3bfc83c8f is container id
  ```
  - To remove container
  ```bash
  docker rm 12c3bfc83c8f
  ```
  - To check particular port in use or not in the host machine (Mac/Linux)
  ```bash 
  lsof -i :5000
  ```
  - To login dokcer hub 
  ```bash
  docker login
  ```
  - To give tag before push into the docker hub
  ```bash
  docker tag node-app:1.0 royal794/node-app:1.0 # royal794 is username and node-app:1.0 is image name
  ```
  - To push into the docker hub
  ```bash
  docker push royal794/node-app:1.0
  ```
  - To up docker compose
  ```bash
  docker compose up -d
  ```
  - To down docker compose
  ```bash
  docker down
  ```

  --------------------------- Now app is ready to run ---------------
