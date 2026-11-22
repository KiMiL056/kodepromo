# syntax=docker/dockerfile:1

FROM php:8.4-cli-alpine
WORKDIR /var/www/html

RUN apk add --no-cache curl

COPY . .

EXPOSE 8080

CMD ["sh", "-c", "php -S 0.0.0.0:${PORT:-8080} -t /var/www/html"]
