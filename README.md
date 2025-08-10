Цей docker-compose піднімає глобальний зворотний проксі з автоматичними SSL-сертифікатами для ваших проєктів.




ssh -L 8081:localhost:81 root@194.31.55.20

	•	ssh — підключення до сервера.
	•	-L 8081:localhost:81 — проброс порту:
	•	8081 — порт на твоєму комп’ютері.
	•	localhost — вказує, що з’єднання піде від сервера на його локальний інтерфейс.
	•	81 — порт NPM на сервері.
	•	root@194.31.55.20 — твій користувач та IP.

    http://localhost:8081



    Створення мережі один раз на сервері
    docker network create proxy-net


    У docker-compose.yml будь-якого іншого проєкту додаєш:
    networks:
        proxy-net:
            external: true




mkdir nginx-proxy-global
cd nginx-proxy-global

# docker-compose.yml
cat << 'EOF' > docker-compose.yml
version: "3.9"

services:
  app:
    image: jc21/nginx-proxy-manager:2
    container_name: npm
    restart: unless-stopped
    ports:
      - "80:80"    # HTTP
      - "443:443"  # HTTPS
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    networks:
      - proxy-net

networks:
  proxy-net:
    external: true
EOF

# README.md

cat << 'EOF' > README.md
# Global Nginx Proxy Manager

Цей docker-compose піднімає глобальний зворотний проксі з автоматичними SSL-сертифікатами для ваших проєктів.

## Команди
```bash
docker network create proxy-net
docker compose up -d