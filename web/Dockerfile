FROM nginx:alpine

LABEL org.opencontainers.image.title="nalupaws-web" \
      org.opencontainers.image.description="Web escaparate estatica de NALUPAWS - Accessories for Dogs & Cats"

# Limpiar contenido por defecto de Nginx y copiar todos los archivos web (incluyendo assets con el logo)
RUN rm -rf /usr/share/nginx/html/*
COPY . /usr/share/nginx/html/

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD wget -q --spider http://127.0.0.1/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
