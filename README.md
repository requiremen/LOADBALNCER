# Loadbalancer-infollion

A simple and efficient Node.js based Load Balancer that routes incoming traffic to multiple backend nodes using an IP hashing algorithm. It includes features for health monitoring and traffic simulation.

## Features
- **IP Hashing**: Ensures consistent routing where a specific IP always maps to the same node (unless the node is down).
- **Health Checks**: Dynamically enable or disable nodes to handle failures.
- **Traffic Simulation**: Test the distribution of requests across available nodes.
- **Random IP Generation**: Utility to simulate diverse incoming traffic.

## Prerequisites
- Node.js (v14 or higher recommended)
- npm (Node Package Manager)

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/requiremen/Loadbalancer-infollion.git
   cd Loadbalancer-infollion
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

## Running the Project

### Development Mode (with Nodemon)
To start the server with auto-reload:
```bash
npm run dev
```

### Normal Mode
```bash
node server.js
```
The server will start on `http://localhost:3000`.

## API Endpoints

### 1. Route Request
Routes a specific IP address to a node.
- **URL**: `/route`
- **Method**: `POST`
- **Body**:
  ```json
  {
    "ip": "192.168.1.1"
  }
  ```

### 2. Update Node Health
Change the status of a node (Node-A, Node-B, or Node-C).
- **URL**: `/health`
- **Method**: `POST`
- **Body**:
  ```json
  {
    "node": "Node-B",
    "status": false
  }
  ```

### 3. Simulate Traffic
Simulate a batch of requests from random IPs.
- **URL**: `/simulate`
- **Method**: `GET`
- **Query Params**: `count` (optional, default: 10)
- **Example**: `/simulate?count=5`

## Test Cases

The project includes a `test.rest` file for testing endpoints using the REST Client extension in VS Code.

### Scenario 1: Basic Routing
- **Input**: `POST /route` with a specific IP.
- **Expected**: The load balancer should return a node name (Node-A, Node-B, or Node-C) based on the IP hash. Repeated requests with the same IP should yield the same node.

### Scenario 2: Node Failure (Health Check)
- **Action**: Disable a node using `POST /health`.
- **Input**: `POST /route` or `GET /simulate`.
- **Expected**: The load balancer should automatically skip the disabled node and route traffic to the next available node in the ring.

### Scenario 3: Traffic Distribution
- **Action**: Run `GET /simulate?count=50`.
- **Expected**: Observe the distribution of requests across all active nodes. The hashing algorithm should provide a relatively balanced distribution for random IPs.

---
*Created as part of the Infollion Load Balancer task.*
