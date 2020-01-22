# Build and run Docker

## Build
docker build -t m4eu-deegree .

### Breakdown
docker build is the command to tell Docker to build an image
-t m4eu-deegree sets the tag name of the Docker image to m4eu-deegree
Finally . tell Docker where to look for the Dockerfile needed for the build (. means current directory)

## Run the image
docker run -p 1234:8080 --name=m4eu m4eu-deegree

### Breakdown
-p 1234:8080 maps port 1234 on our host machine to port 8080 of the Docker container
--name=m4eu gives our container the name m4eu 
and finally, m4eu-deegree is the name we gave it in docker build -t <name_of_image>

