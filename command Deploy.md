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

نکات مهم برای Production
از nginx:alpine یا nginx:1.xx استفاده کنید (کوچک‌تر)

Resource limits تنظیم کنید

Probes فعال کنید

Readiness/Liveness برای health check

Horizontal Pod Autoscaler فعال کنید

Ingress controller (nginx-ingress یا traefik) نصب کنید

---


# کلون یا ساخت پوشه
mkdir nginx-docker && cd nginx-docker

# فایل‌ها را کپی کنید
# (docker-compose.yml, nginx.conf, html/index.html)

# راه‌اندازی
docker compose up -d

# لاگ‌ها
docker compose logs -f nginx

# وضعیت
docker compose ps

# تست
curl http://localhost

# متوقف کردن
docker compose down

# پاک کردن volumes (داده‌ها)
docker compose down -v


---


