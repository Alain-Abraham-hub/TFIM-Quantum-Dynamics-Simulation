# Docker Setup Guide for TFIM Quantum Dynamics Simulator

This project is now containerized with Docker for easy environment setup and execution.

## Prerequisites

- Docker installed on your system
- Docker Compose (comes with Docker Desktop)

## Quick Start

### Option 1: Using Docker Compose (Recommended)

The simplest way to run the project:

```bash
docker-compose up
```

This will:
1. Build the Docker image
2. Start a Jupyter notebook server on `http://localhost:8888`
3. Mount your project files for live editing

Access the notebook in your browser at: **http://localhost:8888**

### Option 2: Using Docker CLI

Build the image:
```bash
docker build -t tfim-simulator .
```

Run the container:
```bash
docker run -p 8888:8888 -v $(pwd):/workspace tfim-simulator
```

Then access Jupyter at: **http://localhost:8888**

## Features

- **Pre-installed dependencies**: All required packages (Qiskit, NumPy, Matplotlib, etc.) are included
- **Live editing**: Changes to notebooks are reflected immediately via volume mounting
- **Jupyter Lab**: Full Jupyter notebook interface available
- **Isolated environment**: Python dependencies don't affect your host system

## Configuration

### Using Your IBM Quantum API Token

When you open the notebook, you'll need to provide your IBM Quantum API token in the "Backend" cell:

```python
service = QiskitRuntimeService(
    channel='ibm_quantum_platform', 
    token='YOUR_API_TOKEN_HERE'
)
```

### Modifying Dependencies

If you need to add Python packages:

1. Add them to `requirements.txt`
2. Rebuild the image: `docker build -t tfim-simulator .`
3. Restart the container

## Stopping the Container

Press `Ctrl+C` in the terminal where `docker-compose up` is running, or:

```bash
docker-compose down
```

## File Structure in Container

```
/workspace/
├── tfim_dynamics_simulator.ipynb
├── README.md
├── LICENSE
└── images/
    └── circuit_diagram.png
```

## Troubleshooting

### Port already in use
If port 8888 is already in use, edit `docker-compose.yml` and change:
```yaml
ports:
  - "8889:8888"  # Maps container port 8888 to host port 8889
```

### Rebuilding without cache
If you need a fresh build:
```bash
docker-compose build --no-cache
docker-compose up
```

### Viewing logs
```bash
docker-compose logs -f
```

## Python Version

The Docker image uses **Python 3.10-slim** for a balance of features and size. To change it, modify the first line of the Dockerfile:

```dockerfile
FROM python:3.11-slim  # or any other version
```

Then rebuild the image.
