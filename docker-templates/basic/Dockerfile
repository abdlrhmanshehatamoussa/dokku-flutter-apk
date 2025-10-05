FROM nginx:alpine


RUN rm /etc/nginx/conf.d/default.conf
RUN rm -rf /usr/share/nginx/html/*

# Copy all files into the Nginx web root
COPY app-release.apk output-metadata.json /usr/share/nginx/html/

# Install jq for JSON parsing
RUN apk add --no-cache jq

WORKDIR /usr/share/nginx/html/

# Extract metadata and rename the APK file accordingly
RUN META_FILE=output-metadata.json && \
    PACKAGE=$(jq -r '.applicationId' $META_FILE) && \
    VERSION_CODE=$(jq -r '.elements[0].versionCode' $META_FILE) && \
    VERSION_NAME=$(jq -r '.elements[0].versionName' $META_FILE) && \
    echo "📦 Package: $PACKAGE" && \
    echo "🏷️ Version Code: $VERSION_CODE" && \
    echo "🔖 Version Name: $VERSION_NAME" && \
    apkfilename=$PACKAGE-$VERSION_CODE-$VERSION_NAME.apk && \
    mv app-release.apk $apkfilename && \
    ln -s $apkfilename /usr/share/nginx/html/index.apk && \
    echo "server { \
    listen 80; \
    server_name _; \
    root /usr/share/nginx/html; \
    index index.apk; \
    location = / { \
    add_header Content-Disposition \"attachment; filename=$apkfilename\"; \
    add_header Access-Control-Allow-Origin \"*\"; \
    try_files /index.apk =404; \
    } \
    }" > /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
