pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'git@github.com:ashishYadavMetacube/jenkinsPythonProject.git'
            }
        }
        stage('Setup Virtual Environment') {
    steps {
        sh '''
        python3 -m venv venv
        source venv/bin/activate
        pip install -r requirements.txt
        '''
    }
}

        stage('Run FastAPI with Ngrok') {
    steps {
        sh '''
        pkill uvicorn || true
        pkill ngrok || true
        nohup uvicorn main:app --host 0.0.0.0 --port 8000 > fastapi.log 2>&1 &
        sleep 10
        nohup ngrok http 8000 > ngrok.log 2>&1 &
        sleep 10
        '''
    }
}

        stage('Show Ngrok URL') {
            steps {
                sh 'curl http://127.0.0.1:4040/api/tunnels'
            }
        }
    }
}
