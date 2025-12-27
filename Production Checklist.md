Production Checklist

✅ nginx:alpine (کوچک‌ترین سایز)
✅ Healthcheck فعال
✅ Resource limits (در صورت نیاز)
✅ Logs به host mount شده
✅ Security headers
✅ Gzip compression
✅ Static cache
✅ Restart policy
✅ Network isolation

---

 ## index.html

 <!DOCTYPE html>
<html>
<head>
    <title>NGINX + Docker Compose</title>
    <style>
        body { font-family: Arial; text-align: center; padding: 50px; }
        .container { max-width: 600px; margin: 0 auto; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🚀 NGINX در Docker Compose</h1>
        <p>سرور با موفقیت راه‌اندازی شد!</p>
        <p>⏰ <span id="time"></span></p>
    </div>
    <script>
        document.getElementById('time').innerText = new Date().toLocaleString('fa-IR');
    </script>
</body>
</html>

---

## Reverse Proxy (Multi-Service)

# docker-compose.multi.yml
services:
  nginx:
    # ... کانفیگ قبلی
    volumes:
      - ./nginx.proxy.conf:/etc/nginx/nginx.conf:ro

  app1:
    image: node:18-alpine
    command: npx serve -s . -l 3000
    networks:
      - webnet

  app2:
    image: httpd:alpine
    networks:
      - webnet

---

## SSL با Let's Encrypt (Production)

# docker-compose.ssl.yml
services:
  nginx:
    image: nginx:alpine
    volumes:
      - /etc/letsencrypt:/etc/letsencrypt:ro  # از certbot
    # ...


---

## Scale و Load Balancing

# Scale کردن
docker compose up -d --scale nginx=3

# یا در docker-compose.yml:
# deploy:
#   replicas: 3


---

## Monitoring و Health Check

# وضعیت health
docker compose ps

# Resource usage
docker stats nginx-main

# داخل کانتینر
docker compose exec nginx sh


---

# دستور سریع یک‌خطی

# همه چیز را یکجا:
mkdir nginx && cd nginx && \
echo '<h1>NGINX OK</h1>' > index.html && \
docker run -d -v $(pwd):/usr/share/nginx/html -p 80:80 --name nginx --restart=unless-stopped nginx:alpine

---

