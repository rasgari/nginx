# nginx


NGINX یک وب سرور متن‌باز، reverse proxy و load balancer فوق‌العاده قدرتمند است که توسط Igor Sysoev در ۲۰۰۴ ساخته شد.

نصب سریع (Ubuntu/Debian)
```
sudo apt update && sudo apt install nginx -y
sudo systemctl enable --now nginx
sudo ufw allow 'Nginx Full'
curl localhost  # تست
```

کارکرد اصلی NGINX در سرور
```
📡 Web Server: Static files (HTML/CSS/JS) با سرعت بالا
🔄 Reverse Proxy: پنهان کردن backend servers
⚖️ Load Balancer: توزیع ترافیک بین سرورها
🛡️ Cache/Proxy: کاهش بار سرور اصلی
🔒 SSL/TLS Termination: مدیریت HTTPS
📊 Rate Limiting: کنترل درخواست‌ها
```

معماری Event-Driven (مزیت اصلی)
```
Apache: Process/Thread per connection → RAM بالا
NGINX: Single thread + Event loop → ۱۰K+ connections
```
کانفیگ اصلی (/etc/nginx/nginx.conf)
```
events { worker_connections 1024; }  # اتصالات همزمان
http {
    server {
        listen 80; server_name example.com;
        location / { root /var/www/html; }
        location /api/ { proxy_pass http://backend:3000; }
    }
}

# تست: nginx -t && systemctl reload nginx
```

Virtual Hosts (Server Blocks)
```
هر دامنه = یک server block
sudo nginx -s reload  # بدون downtime
```

Load Balancing مثال

```
upstream backend {
    server app1:3000 weight=3;
    server app2:3000 weight=2;
}
server {
    location / { proxy_pass http://backend; }
}

```

Performance Tuning حرفه‌ای
```
worker_processes auto;  # = CPU cores
worker_connections 4096;
gzip on; keepalive_timeout 65;
sendfile on; tcp_nopush on;
```

SSL/HTTPS (Let’s Encrypt)
```
certbot --nginx -d example.com
# خودکار کانفیگ می‌شود
```

لاگ‌ها و Monitoring
```
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
nginx -V  # کامپایل flags
```

Docker/K8s (DevOps)
```
# docker-compose.yml
nginx:
  image: nginx:alpine
  volumes: ['./html:/usr/share/nginx/html']

```

مقایسه با Apache
ویژگی	NGINX	Apache
Static Files	⭐⭐⭐⭐⭐	⭐⭐⭐
Dynamic (PHP)	⭐⭐⭐	⭐⭐⭐⭐⭐
RAM Usage	کم	زیاد
Connections	۵۰K+	۱K-۵K
Config	JSON-like	XML-like

دستورات حیاتی Admin
```
nginx -t          # تست کانفیگ
nginx -s reload   # reload بدون downtime
nginx -s stop     # توقف graceful
ps aux | grep nginx  # پروسه‌ها
netstat -tulpn | grep :80  # پورت‌ها
```

Security Hardening
```
server_tokens off;  # مخفی کردن نسخه
limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;
add_header X-Frame-Options DENY;
```

Metrics و Monitoring
```
nginx -V 2>&1 | grep -o with-http_stub_status_module
stub_status;  # /nginx_status
Prometheus + Grafana exporter
```
