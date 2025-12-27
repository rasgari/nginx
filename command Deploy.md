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


---

Scale و Autoscaling

# Scale دستی
kubectl scale deployment nginx-deployment --replicas=5

# Horizontal Pod Autoscaler
kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=2 --max=10

---

Troubleshooting سریع


# لاگ pod ها
kubectl logs -l app=nginx -f

# Describe pod
kubectl describe pod <pod-name>

# Events
kubectl get events --sort-by='.lastTimestamp'

# Test connectivity
kubectl exec -it <pod-name> -- curl localhost

---
