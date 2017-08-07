FROM udacity/swift-base:latest

RUN apt-get update; \
        apt-get install -y mysql-client; \
        apt-get autoremove -y; \
        rm -rf /var/lib/apt/lists/*
