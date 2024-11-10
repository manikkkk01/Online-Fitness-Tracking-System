Online Fitness Tracking System

This is an online fitness tracking system designed to help users log their workouts, track nutrition, and manage their health goals. The application allows users to input their workout plans, track calories, and manage macronutrient information for a healthier lifestyle.

Tech Stack
Frontend: HTML, CSS, JavaScript, React/Angular
Backend: Java, Spring Boot, Spring Security
Database: MySQL, Calorie & Macronutrient Database
Deployment: AWS EC2, Docker, GitHub CI/CD
Project Setup
Follow the steps below to run the project locally and deploy it.

1. Clone the Repository
bash
Copy code
git clone https://github.com/yourusername/fitness-tracking-system.git
cd fitness-tracking-system
2. Frontend
This project uses React for the frontend. To get started, follow these steps:

Navigate to the frontend/ directory:

bash
Copy code
cd frontend
Install dependencies:

bash
Copy code
npm install
Start the React development server:

bash
Copy code
npm start
The frontend should now be running on http://localhost:3000.

3. Backend (Spring Boot)
The backend is built using Spring Boot. To run the Spring Boot application locally:

Navigate to the backend/ directory:

bash
Copy code
cd backend
Build the Spring Boot application using Maven:

bash
Copy code
mvn clean install
Run the application:

bash
Copy code
mvn spring-boot:run
The backend will be running on http://localhost:8080.

4. Database Setup
The project uses MySQL for storing user data, workout plans, and nutrition logs. To set up the database:

Install MySQL on your machine (or use AWS RDS for production).

Create the necessary database and tables:

sql
Copy code
CREATE DATABASE fitness_tracker_db;
USE fitness_tracker_db;
Import the SQL schema files from the backend/src/main/resources directory.

Example:

bash
Copy code
mysql -u root -p < schema.sql
5. Dockerization
To run the application inside a Docker container:

Create a Docker image for the Spring Boot application:

bash
Copy code
docker build -t fitness-tracking-system .
Run the Docker container:

bash
Copy code
docker run -d -p 8080:8080 fitness-tracking-system
6. Deploying to AWS EC2
Follow these steps to deploy your application to AWS:

Create an EC2 Instance:

Launch an EC2 instance with an Ubuntu image.
SSH into your EC2 instance.
Install Docker on EC2:

bash
Copy code
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
Push the Docker image to Docker Hub:

bash
Copy code
docker login
docker tag fitness-tracking-system yourdockerhubusername/fitness-tracking-system
docker push yourdockerhubusername/fitness-tracking-system
Pull the Docker image on EC2:

bash
Copy code
docker pull yourdockerhubusername/fitness-tracking-system
Run the Docker container on EC2:

bash
Copy code
docker run -d -p 8080:8080 yourdockerhubusername/fitness-tracking-system
Your application will now be accessible from your EC2 public IP at port 8080.

7. CI/CD with GitHub Actions
This project uses GitHub Actions to automate the build and deployment process. The CI/CD pipeline is defined in .github/workflows/main.yml.

Workflow Overview:
On every push to the main branch, GitHub Actions will:
Build the Spring Boot application.
Build a Docker image.
Push the image to Docker Hub.
Deploy the image to your AWS EC2 instance.
GitHub Secrets Configuration:
Make sure to add the following secrets to your GitHub repository:

DOCKER_USERNAME: Your Docker Hub username.
DOCKER_PASSWORD: Your Docker Hub password.
EC2_PRIVATE_KEY: The private key for your EC2 instance.
Workflow Example:
yaml
Copy code
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          java-version: '11'
          distribution: 'adoptopenjdk'

      - name: Build with Maven
        run: mvn clean install -DskipTests

      - name: Build Docker image
        run: |
          docker build -t fitness-tracking-system .

      - name: Push Docker image to Docker Hub
        run: |
          docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
          docker tag fitness-tracking-system yourdockerhubusername/fitness-tracking-system
          docker push yourdockerhubusername/fitness-tracking-system

  deploy:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy to EC2
        run: |
          ssh -i ${{ secrets.EC2_PRIVATE_KEY }} ubuntu@your-ec2-public-ip << 'EOF'
            docker pull yourdockerhubusername/fitness-tracking-system
            docker run -d -p 8080:8080 yourdockerhubusername/fitness-tracking-system
          EOF
Contributing
Feel free to fork this repository, submit issues, and open pull requests. If you have suggestions or improvements, please create a new issue.

License
This project is licensed under the MIT License - see the LICENSE file for details.
