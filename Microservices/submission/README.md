# Microservices Deployment on Kubernetes using Minikube

## 1. Project Overview

This project deploys four Node.js microservices on a local Kubernetes cluster using Minikube.

The four services are:

- User Service
- Product Service
- Order Service
- Gateway Service

Each microservice is packaged into a Docker image and deployed using Kubernetes Deployments. Kubernetes ClusterIP Services provide internal communication between the microservices.

## 2. Technologies Used

- Docker
- Kubernetes
- Minikube
- kubectl
- Node.js
- Express.js
- Axios

## 3. Project Structure

```text
submission/
├── deployments/
│   ├── user-service.yaml
│   ├── product-service.yaml
│   ├── order-service.yaml
│   └── gateway-service.yaml
├── services/
│   ├── user-service.yaml
│   ├── product-service.yaml
│   ├── order-service.yaml
│   └── gateway-service.yaml
├── ingress/
├── screenshots/
│   ├── pods.png
│   ├── logs.png
│   └── service-test.png
└── README.md
```

## 4. Start Minikube

Start the Kubernetes cluster:

```powershell
minikube start --driver=docker --cpus=4 --memory=4096
```

Check the cluster:

```powershell
kubectl get nodes
```

## 5. Build Docker Images

Build all four microservice images inside Minikube:

```powershell
minikube image build -t user-service:1.0 .\Microservices\user-service
minikube image build -t product-service:1.0 .\Microservices\product-service
minikube image build -t order-service:1.0 .\Microservices\order-service
minikube image build -t gateway-service:1.0 .\Microservices\gateway-service
```

## 6. Deploy Kubernetes Services and Deployments

Apply the Kubernetes Service configurations:

```powershell
kubectl apply -f submission/services/
```

Apply the Deployment configurations:

```powershell
kubectl apply -f submission/deployments/
```

## 7. Verify Deployment

Check the Pods:

```powershell
kubectl get pods
```

Check the Deployments:

```powershell
kubectl get deployments
```

Check the Services:

```powershell
kubectl get services
```

Check service endpoints:

```powershell
kubectl get endpoints
```

All four microservices should be running and their Services should have corresponding Pod endpoints.

## 8. Test Microservice Communication

Forward the Gateway Service port:

```powershell
kubectl port-forward service/gateway-service 3003:3003
```

In another terminal, test the APIs:

**User Service**

```powershell
curl.exe http://localhost:3003/api/users
```

**Product Service**

```powershell
curl.exe http://localhost:3003/api/products
```

**Order Service**

```powershell
curl.exe http://localhost:3003/api/orders
```

The Gateway Service forwards requests to the appropriate internal microservice using Kubernetes Service DNS names.

## 9. Order Creation

The Order Service supports creating orders using a POST request.

Example using PowerShell:

```powershell
$body = @{
    userId = 1
    productId = 1
} | ConvertTo-Json -Compress

Invoke-RestMethod -Uri "http://localhost:3003/api/orders" `
    -Method POST `
    -ContentType "application/json" `
    -Body $body
```

Orders are stored in memory by the application and are not persistent across Pod restarts.

## 10. Logs and Troubleshooting

View Gateway logs:

```powershell
kubectl logs deployment/gateway-service
```

View individual service logs:

```powershell
kubectl logs deployment/user-service
kubectl logs deployment/product-service
kubectl logs deployment/order-service
```

Check all Kubernetes resources:

```powershell
kubectl get pods,services,deployments
```

## 11. Conclusion

Successfully deployed four Node.js microservices on Kubernetes using Minikube.

Verified that:
- All four Pods are running and ready.
- All four Kubernetes Services are configured as ClusterIP.
- Each Service has a corresponding Pod endpoint.
- The Gateway Service successfully communicates with the User, Product, and Order Services.
- API endpoints return valid JSON responses.
- Order creation and retrieval work successfully.
