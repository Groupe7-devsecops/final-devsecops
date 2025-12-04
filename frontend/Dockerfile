FROM nginx:alpine


RUN addgroup -S appgroup && adduser -S appuser -G appgroup


RUN rm /etc/nginx/conf.d/default.conf


COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY . /usr/share/nginx/html


RUN mkdir -p /var/cache/nginx /var/run /run /var/log/nginx \
    && chown -R appuser:appgroup /var/cache/nginx /var/run /run /var/log/nginx /usr/share/nginx/html

USER appuser

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
