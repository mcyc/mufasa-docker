# mufasa-docker
Docker containers for stable versions of MUFASA.  
Each version is encapsulated in its own directory.

---

## Available Versions
- `latest` (points to the most recent stable version of MUFASA)
- [v1.4.0](v1.4.0/README.md)

---

## Usage

### Pulling a Container
To pull a specific version of MUFASA:

    docker pull your-username/mufasa:v1.4.0

To pull the `latest` version of MUFASA:

    docker pull your-username/mufasa:latest

### Running the Container
To run the MUFASA Docker container, map port `8888` inside the container to a host port (e.g., `8888`):

    docker run -it -p 8888:8888 mufasa:v1.4.0

For the `latest` version:

    docker run -it -p 8888:8888 mufasa:latest

Access the application in your browser at:
    http://localhost:8888

---

## How to Use Docker Compose

### Prerequisites
Make sure you have Docker and Docker Compose installed on your system. If not, you can follow the [Docker installation guide](https://docs.docker.com/get-docker/) and [Docker Compose installation guide](https://docs.docker.com/compose/install/).

### Running the Container
1. Clone this repository:

       git clone https://github.com/your-username/mufasa-docker.git
       cd mufasa-docker

2. Start the container with Docker Compose:

       docker-compose up -d

3. Access the application in your browser:

       http://localhost:8888

4. To stop the container:

       docker-compose down

---

### Running Specific Versions
If you want to build and run a specific version of MUFASA, navigate to the version directory (e.g., `v1.4.0`) and use Docker commands directly.

Example:

       cd v1.4.0
       docker build -t mufasa:v1.4.0 .
       docker run -it -p 8888:8888 mufasa:v1.4.0

---

## Publishing to CANFAR Harbor

### Tagging the Container
To tag the container for publishing on CANFAR Harbor:

       docker tag mufasa:v1.4.0 images.canfar.net/your-project/mufasa:v1.4.0
       docker tag mufasa:v1.4.0 images.canfar.net/your-project/mufasa:latest

### Logging into CANFAR Harbor
Log in to CANFAR Harbor:

       docker login images.canfar.net

### Pushing the Container
To publish the container:

       docker push images.canfar.net/your-project/mufasa:v1.4.0
       docker push images.canfar.net/your-project/mufasa:latest

---

## License
This project is licensed under the [MIT License](LICENSE).
