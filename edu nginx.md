# edu nginx

بعد از اجرا Deployment با ۳ Replica (۳ Pod)
اجرا:
```
kubectl apply -f nginx-deployment.yaml
```
بررسی:
```
kubectl get pods -l app=nginx
```

خروجی چیزی شبیه این می‌بینی:
```
nginx-deployment-xxx   Running
nginx-deployment-yyy   Running
nginx-deployment-zzz   Running
```
2️⃣ (اختیاری ولی مهم) Service برای دسترسی به nginx

بدون Service دسترسی درست نداری. این ClusterIP Service ترافیک رو بین ۳ Pod لودبالانس می‌کنه:
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP
```

```
kubectl apply -f nginx-service.yaml
```
تست داخل کلاستر:
```
kubectl exec -it <any-pod> -- curl nginx-service
```
3️⃣ اگر روی NodePort یا بیرون کلاستر می‌خوای
```
NodePort:
type: NodePort
ports:
- port: 80
  targetPort: 80
  nodePort: 30080
```

دسترسی:
```
http://<NODE-IP>:30080
```
