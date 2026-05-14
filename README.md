# README
### Simple HTTP Compute Service (Go + Docker)

This project implements a lightweight HTTP service written in Go that performs basic arithmetic operations and provides a health check endpoint.  
The application is containerized using Docker and built with a multi-stage Docker build for a smaller final image.

---

## 1. How to Build the Docker Image

Open a terminal in the project directory and run:

docker build -t go-service .

This command builds the Docker image named:

go-service

---

## 2. How to Run the Container

Run the service using:

docker run -p 8080:8080 go-service

The application will start and listen on port 8080.

---

## 3. Port Configuration

The service uses the following port configuration:

Internal container port:
8080

External host port:
8080

Service address:

http://localhost:8080

---

## 4. Connecting from Host to VM

If the Docker container is running inside a Linux Virtual Machine, the host system must be able to access the VM network.

Recommended configuration: NAT with Port Forwarding.

Steps in VirtualBox:

1. Open VM Settings
2. Go to Network
3. Select Adapter → NAT
4. Open Port Forwarding
5. Add a rule with the following configuration

Name: http  
Protocol: TCP  
Host Port: 8080  
Guest Port: 8080  

After configuring this, the service can be accessed from the host machine using:

http://127.0.0.1:8080

Alternatively, if using Bridged Adapter mode, the service can be accessed using the VM's IP address:

http://VM_IP_ADDRESS:8080

---

## 5. Available Endpoints

The service exposes two endpoints:

Health endpoint:
/health

Compute endpoint:
/compute

---

## 6. Health Check Example

Request:

curl http://localhost:8080/health

Response:

{"status":"ok"}

---

## 7. Compute Operation Examples

Addition:

curl "http://localhost:8080/compute?op=add&a=10&b=5"

Response:

{"operation":"add","a":10,"b":5,"result":15}


Subtraction:

curl "http://localhost:8080/compute?op=sub&a=10&b=3"


Multiplication:

curl "http://localhost:8080/compute?op=mul&a=6&b=7"


Division:

curl "http://localhost:8080/compute?op=div&a=12&b=3"

---

## 8. Error Handling Examples

Missing parameters:

curl "http://localhost:8080/compute?op=add&a=10"

Example response:

{"error":"missing parameters: op, a, b are required"}


Invalid operation:

curl "http://localhost:8080/compute?op=test&a=1&b=2"

Example response:

{"error":"invalid operation: test"}


Non numeric input:

curl "http://localhost:8080/compute?op=add&a=hello&b=5"

Example response:

{"error":"parameter a must be numeric"}


Division by zero:

curl "http://localhost:8080/compute?op=div&a=10&b=0"

Example response:

{"error":"division by zero"}


Invalid HTTP method:

curl -X POST http://localhost:8080/health

Example response:

{"error":"method not allowed"}

---

## 9. Implementation Details

The application is implemented using Go standard libraries including:

net/http  
encoding/json  

The service validates all inputs and returns proper HTTP error responses.

The Dockerfile uses a multi-stage build to compile the Go binary and produce a small final container image based on Alpine Linux.

---

## Summary

This project demonstrates:

HTTP server development in Go  
REST-style API endpoints  
JSON request and response handling  
Robust error handling  
Docker containerization  
Multi-stage Docker builds  
Host to VM networking configuration
