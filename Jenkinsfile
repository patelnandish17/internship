pipeline {
    agent any

    environment {
        // Securely inject the .env credential file
        ENV_FILE = credentials('ENV')
    }

    stages {
        stage('Setup and Environment') {
            steps {
                echo '=== STEP 1: Setting up Environment ==='
                // Copy the secure environment variables file to both frontend and backend
                bat 'copy "%ENV_FILE%" .env'
                bat 'copy "%ENV_FILE%" "Lang graph\\.env"'
                echo '=== Environment files copied successfully ==='
                
                // Verify Docker is accessible
                echo '=== Verifying Docker daemon connection ==='
                bat 'docker ps'
            }
        }

        stage('Front-end Pipeline') {
            steps {
                echo '=== STEP 2: Front-end Pipeline ==='
                echo 'Installing Front-end Dependencies...'
                bat 'npm install'
                
                echo 'Running Front-end Validations / Tests...'
                bat 'npm run test'
                
                echo 'Building Front-end Application...'
                bat 'npm run build'
                
                echo 'Creating Front-end Docker Image...'
                bat 'docker build -t frontend:latest .'
            }
        }

        stage('Back-end Pipeline') {
            steps {
                echo '=== STEP 3: Back-end Pipeline ==='
                echo 'Creating Virtual Environment and Installing Dependencies...'
                bat 'cd "Lang graph" && python -m venv venv && .\\venv\\Scripts\\pip install --upgrade pip && .\\venv\\Scripts\\pip install -r requirements.txt'
                
                echo 'Executing Back-end Tests...'
                bat 'cd "Lang graph" && set PYTHONPATH=c:\\Users\\Likith M V\\Downloads\\placement-compass-16-main\\Lang graph&& .\\venv\\Scripts\\python -m pytest tests/'
                
                echo 'Building Services...'
                bat 'cd "Lang graph" && .\\venv\\Scripts\\python -c "import main; print(\'Backend compilation verified successfully!\')"'
                
                echo 'Creating Back-end Docker Image...'
                bat 'docker build -t backend:latest "./Lang graph"'
            }
        }

        stage('Agentic Orchestration Pipeline') {
            steps {
                echo '=== STEP 4: Agentic Orchestration Pipeline ==='
                echo 'Setting up Multi-agent Orchestration...'
                // Check if the graph loads and compiles without errors
                bat 'cd "Lang graph" && set PYTHONPATH=c:\\Users\\Likith M V\\Downloads\\placement-compass-16-main\\Lang graph&& .\\venv\\Scripts\\python -c "from main import graph; print(\'Orchestration graph compiled successfully!\')"'
                
                echo 'Initializing Docker Compose Services...'
                bat 'docker-compose down'
                bat 'docker-compose up -d'
                
                echo 'Verifying Running Containers...'
                bat 'docker ps'
                
                echo 'Running Agentic Workflow Smoke Test...'
                // Run the smoke test to verify execution workflow
                bat 'cd "Lang graph" && set PYTHONPATH=c:\\Users\\Likith M V\\Downloads\\placement-compass-16-main\\Lang graph&& .\\venv\\Scripts\\python smoke_test.py'
            }
        }
    }

    post {
        always {
            echo '=== Cleaning up running containers ==='
            bat 'docker-compose down'
        }
        success {
            echo '=== Pipeline Execution Succeeded! ==='
        }
        failure {
            echo '=== Pipeline Execution Failed! ==='
        }
    }
}
