# Docker Compose Todo Application

This project is designed to Dockerize a simple Todo application consisting of a backend (Node.js) and a frontend (React) application. You can easily set up and manage the application using Docker and Docker Compose.

## Project Structure

```
.
├── docker-compose.yml
├── backend/
│   ├── Dockerfile
│   ├── index.js
│   ├── package.json
│   └── ...
└── frontend/
    ├── Dockerfile
    ├── package.json
    └── ...
```

## Docker Compose

The [`docker-compose.yml`](docker-compose.yml) file defines all services (backend and frontend) of the application and configures how they work together.

```yaml
version: "3"

services:
  backend:
    build: ./backend
    ports:
      - "4000:4000"

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
```

### Services

-   **`backend`**: Node.js based backend service.
    -   `build: ./backend`: The Docker image for the backend service is built using the [`Dockerfile`](backend/Dockerfile) in the `./backend` directory.
    -   `ports: "4000:4000"`: Maps the container's port 4000 to the host machine's port 4000. The backend application will be accessible via this port.
-   **`frontend`**: React based frontend service.
    -   `build: ./frontend`: The Docker image for the frontend service is built using the [`Dockerfile`](frontend/Dockerfile) in the `./frontend` directory.
    -   `ports: "3000:3000"`: Maps the container's port 3000 to the host machine's port 3000. The frontend application will be accessible via this port.
    -   `depends_on: - backend`: Ensures that the backend service is running before the frontend service starts.

## Dockerfiles

### Backend Dockerfile (`backend/Dockerfile`)

This [`Dockerfile`](backend/Dockerfile) creates a Docker image for the Node.js backend application.

```dockerfile
FROM node:18-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install && npm install uuid
COPY . .

EXPOSE 4000
CMD ["node", "index.js"]
```

-   `FROM node:18-alpine`: Uses the official Node.js 18 Alpine Linux-based image. This provides a smaller image size.
-   `WORKDIR /app`: Sets the working directory inside the container to `/app`.
-   `COPY package*.json ./`: Copies `package.json` and `package-lock.json` files to the working directory.
-   `RUN npm install && npm install uuid`: Installs project dependencies and the `uuid` package.
-   `COPY . .`: Copies the rest of the project files to the working directory.
-   `EXPOSE 4000`: Specifies that the application listens on port 4000.
-   `CMD ["node", "index.js"]`: Starts the backend application by running the `node index.js` command when the container starts.

### Frontend Dockerfile (`frontend/Dockerfile`)

This [`Dockerfile`](frontend/Dockerfile) creates a Docker image for the React frontend application.

```dockerfile
FROM node:18-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

EXPOSE 3000
CMD ["npm", "start"]
```

-   `FROM node:18-alpine`: Uses the official Node.js 18 Alpine Linux-based image.
-   `WORKDIR /app`: Sets the working directory inside the container to `/app`.
-   `COPY package*.json ./`: Copies `package.json` and `package-lock.json` files to the working directory.
-   `RUN npm install`: Installs project dependencies.
-   `COPY . .`: Copies the rest of the project files to the working directory.
-   `EXPOSE 3000`: Specifies that the application listens on port 3000.
-   `CMD ["npm", "start"]`: Starts the frontend application by running the `npm start` command when the container starts.

## Running the Application

1.  Navigate to the project directory:
    ```bash
    cd /path/to/your/project
    ```
2.  Start the application using Docker Compose:
    ```bash
    docker-compose up --build
    ```
    This command will build the Docker images and start the services according to the definitions in the `docker-compose.yml` file.

3.  Access the application:
    -   Frontend: `http://localhost:3000`
    -   Backend: `http://localhost:4000`

## Stopping the Application

To stop the application and remove the created containers, run the following command in the project directory:

```bash
docker-compose down