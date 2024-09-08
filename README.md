# Platform-Engineer-Challenge
My solution to the given Admin Recruitment Challenge (https://github.com/bdu-xpand-it/BDU-Recruitment-Challenges/wiki/Admin-Recruitment-Challenge)

# Apache Tomcat container deployment with Oracle Linux image

This repository contains a Dockerfile to create a Docker image based on Oracle Linux 9. The image includes Java 22, and Apache Tomcat 10.1.28, along with SSL configuration.

## Prerequisites

- Docker installed on your machine.

## Building the Docker Image

To build the Docker image, run the following command inside the project's directory containing the Dockerfile:

docker build -t oraclelinux-tomcat .

(Note that oraclelinux-tomcat is an arbitrary name)

Running the Docker Container

To run the Docker container, use the following command:

docker run -d -p 4041:4041  oraclelinux-tomcat

Included Software

    Oracle Linux 9: Base image.
    Java 22: Downloaded and installed manually.
    Apache Tomcat 10.1.28: Downloaded and installed manually.
    SSL Configuration: Includes private and public keys, certificate signing request (CSR), and keystore.

Environment Variables

    JAVA_HOME: Path to Java 22 installation.
    PATH: Includes Java 22 binaries.
    SSL_PATH: Path to SSL configuration.
    CATALINA_HOME: Path to Tomcat installation.

Exposed Ports

    4041: Tomcat server port.

SSL Configuration

The Dockerfile includes steps to generate a certificate signing request (CSR), sign the certificate, and create a keystore. The SSL configuration files are copied into the container.
Custom Configuration

    server.xml: Custom Tomcat server configuration.
    sample.war: Sample web application deployed to Tomcat.

Commands

    Build Image: docker build -t oraclelinux-tomcat .
    Run Container: docker run -d -p 4041:4041 oraclelinux-tomcat

Notes

    Ensure that the private_key.pem and public_key.pem files are present in the same directory as the Dockerfile.
    The server.xml and sample.war files should also be present in the same directory as the Dockerfile.

Author: Frederico Teixeira
