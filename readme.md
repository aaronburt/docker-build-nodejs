Optimised Docker build file

```
# Build a builder docker container - this container will be disregarded once contents are extracted
FROM node:16 as Builder
WORKDIR /app
COPY package*.json index.js ytdl.js yt-dlp ./
RUN npm install

# Build the deployment apline container
from alpine:latest
# Install requirements for the application to run
RUN apk add --update nodejs
RUN apk add python3
WORKDIR /app
# Extract the contents of builder
COPY --from=Builder /app /app

# Expose access to port 8080 for index.js
EXPOSE 8080

# Run index.js
CMD [ "node", "index.js" ]
```
