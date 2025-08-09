Created 2 Dockerfiles:

1- Multi layer with dockerignore
2- Single layer

For Multi layer:

FROM springcommunity/spring-petclinic-rest

# Build arguments
ARG ENV=dev
ARG APP_VERSION=1.0

# Environment configuration
ENV SPRING_PROFILES_ACTIVE=$ENV \
    APP_VERSION=$APP_VERSION

# Working directory setup
WORKDIR /app

# Copy application files (respects .dockerignore)
COPY . .

# Installation in multiple layers
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y vim
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Application execution
ENTRYPOINT ["java", "-jar", "spring-petclinic-rest.jar"]
CMD ["--server.port=8080"]

.dockerignore:

# Build artifacts
target/
bin/
build/

# Version control
.git/
.gitignore

# IDE files
.idea/
*.iml

# Environment files
.env
*.properties

# Logs and temp files
*.log
*.tmp
*.swp

# Documentation
*.md
LICENSE

One layer:

FROM springcommunity/spring-petclinic-rest

# Build configuration
ARG ENV=dev
ARG APP_VERSION=1.0

# Environment setup
ENV SPRING_PROFILES_ACTIVE=$ENV \
    APP_VERSION=$APP_VERSION

# Directory structure
WORKDIR /app

COPY . .

# Combined installation and cleanup
RUN apt-get update && \
    apt-get install -y curl vim && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Execution configuration
ENTRYPOINT ["java", "-jar", "spring-petclinic-rest.jar"]
CMD ["--server.port=8080"]
