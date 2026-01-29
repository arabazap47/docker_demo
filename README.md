THis Is final of my jira, docker github
java to docker to aws
On Your Local Machine (Windows)

Create project:

mkdir java-docker
cd java-docker


Create App.java:

public class App {
    public static void main(String[] args) {
        System.out.println("Java App Running inside Docker!");
    }
}


Compile it:

javac --release 17 App.java

You should now have:

App.java
App.class


Create Dockerfile:

FROM eclipse-temurin:17-jre
WORKDIR /app
COPY App.class .
CMD ["java", "App"]


Build image locally:

docker build -t java-app .


Run locally:

docker run java-app


Output should be:

Java App Running inside Docker!


This proves the container works.

☁ Move to EC2

From inside java-docker on your laptop:

scp -r . ubuntu@54.146.161.248:/home/ubuntu/java-docker


Login to EC2:

ssh ubuntu@54.146.161.248
cd java-docker


Build on EC2:

docker build -t java-app .


Run on EC2:

docker run java-app


git actions ***********************************************************************************************************
name: Build Employee App

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Build Docker Image
        run: docker build -t employee-app .


maven yaml***************************************************************************************
name: Java CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up Java
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Build with Maven
        run: mvn clean package

You will see on EC2 terminal:

Java App Running inside Docker!
