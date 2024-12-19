# mufasa-docker
Docker containers for stable versions of MUFASA.  
Each version is encapsulated in its own directory.

---

## Available Versions
- [v1.4.0](v1.4.0/README.md)

---

## Usage
To pull a specific version of MUFASA:

    docker pull your-username/mufasa:v1.4.0

---

## Running MUFASA Docker Containers

To run the MUFASA Docker container, map port `5000` inside the container to port `5001` on the host:

    docker run -it -p 5001:5000 mufasa:v1.4.0

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

3. Access the application in your browser at:
    
        http://localhost:5001

4. To stop the container:
    
        docker-compose down

---

### Running Specific Versions
If you want to build and run a specific version of MUFASA, navigate to the version directory (e.g., `v1.4.0`) and use Docker commands directly.

Example:
    
        cd v1.4.0
        docker build -t mufasa:v1.4.0 .
        docker run -it -p 5001:5000 mufasa:v1.4.0

---

## License
This project is licensed under the [MIT License](LICENSE).
