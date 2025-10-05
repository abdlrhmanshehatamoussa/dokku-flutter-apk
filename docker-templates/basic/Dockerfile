FROM nginx:alpine

# Copy all files into the Nginx web root
COPY . /usr/share/nginx/html/

# Find the first .apk file and create a symlink named 'index.apk' pointing to it
RUN apk_file=$(find /usr/share/nginx/html -maxdepth 1 -type f -name "*.apk" | head -n 1) && \
    if [ -z "$apk_file" ]; then \
        echo "❌ No APK file found in build context"; exit 1; \
    else \
        ln -s "$(basename "$apk_file")" /usr/share/nginx/html/index.apk && \
        echo "✅ Serving $(basename "$apk_file") as index.apk"; \
    fi

# Configure Nginx to serve that symlink when visiting "/"
RUN echo 'server { \
    listen 80; \
    server_name _; \
    root /usr/share/nginx/html; \
    index index.apk; \
    location = / { \
        add_header Content-Disposition "attachment"; \
        add_header Access-Control-Allow-Origin "*"; \
        try_files /index.apk =404; \
    } \
}' > /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
