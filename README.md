# Multi-Container Application with Helm

This is a simple multi-container application that demonstrates how to deploy multiple services using Helm on Kubernetes.

## Prerequisites

- Kubernetes cluster (Minikube, Docker Desktop with Kubernetes, or cloud-based)
- `kubectl` configured to communicate with your cluster
- Helm 3 installed

## Components

1. **Frontend**: Nginx web server
   - Serves static content
   - Routes requests to the backend

2. **Backend**: Node.js server
   - REST API endpoints
   - Integrated with Redis
   - Health checks implemented
   - Environment: Production

3. **Redis**: In-memory data store
   - Used for caching and session management
   - Configurable port settings

## Installation

1. Add the chart repository (if needed):
   ```bash
   helm repo add myrepo https://charts.mycompany.com/
   ```

2. Install the chart:
   ```bash
   helm install my-release ./
   ```

## Configuration

You can customize the deployment by overriding values in `values.yaml` or using the `--set` flag. Common configuration options include:

```bash
# Set frontend replicas
helm install my-release ./ --set frontend.replicaCount=2

# Set backend replicas
helm install my-release ./ --set backend.replicaCount=3

# Configure Redis port
helm install my-release ./ --set redis.service.port=6379
```

## Accessing the Application

### Frontend
To access the frontend service, use port-forwarding:
```bash
kubectl port-forward svc/my-release-multi-container-app-frontend 8080:80
```
Then open http://localhost:8080 in your browser.

### Backend API
The backend service runs on port 3000. To access it directly:
```bash
kubectl port-forward svc/my-release-multi-container-app-backend 3001:3000
```
You can then access API endpoints at http://localhost:3001

## Uninstalling the Chart

To uninstall/delete the deployment:

```bash
helm delete my-release
```

## Health Checks

The backend service includes both liveness and readiness probes:
- Liveness probe: /api/health (every 10 seconds)
- Readiness probe: /api/health (every 5 seconds)

These probes help Kubernetes determine the health of the backend containers and manage the service's availability.

## License
[MIT](LICENSE)
