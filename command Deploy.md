# اعمال Deployment
kubectl apply -f nginx-deployment.yaml

# اعمال Service
kubectl apply -f nginx-service.yaml

# چک وضعیت
kubectl get deployments
kubectl get pods -l app=nginx
kubectl get services

# Port Forward برای تست محلی
kubectl port-forward service/nginx-service 8080:80

# دسترسی: http://localhost:8080
