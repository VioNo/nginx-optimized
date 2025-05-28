FROM nginx:alpine

# Устанавливаем bash и необходимые утилиты ДО создания пользователя
RUN apk add --no-cache bash curl

# Удаляем проблемные скрипты
RUN rm -rf /docker-entrypoint.d/

# Создаем пользователя и настраиваем окружение
RUN adduser -D -u 1000 -s /bin/bash user_octov && \
    mkdir -p /var/cache/nginx/client_temp \
             /var/run/nginx \
             /etc/nginx/ssl \
             /home/user_octov && \
    chown -R user_octov:user_octov /var/cache/nginx \
                                   /var/run/nginx \
                                   /etc/nginx \
                                   /usr/share/nginx/html \
                                   /home/user_octov && \
    chmod -R 755 /var/cache/nginx \
                 /var/run/nginx && \
    chmod 750 /etc/nginx/ssl

# Копируем файлы
COPY --chown=user_octov:user_octov ssl/ /etc/nginx/ssl/
COPY --chown=user_octov:user_octov html/ /usr/share/nginx/html/
COPY --chown=user_octov:user_octov nginx.conf /etc/nginx/nginx.conf
COPY --chown=user_octov:user_octov default.conf /etc/nginx/conf.d/default.conf

# Фиксируем права
RUN chmod 600 /etc/nginx/ssl/key.pem && \
    chmod 644 /etc/nginx/ssl/cert.pem

USER user_octov
WORKDIR /home/user_octov
EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;"]