


pipeline {
    agent any
    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"  // Add Homebrew's bin to PATH
    }
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
        # Kill any existing processes to avoid conflicts
        pkill -f uvicorn || true
        pkill -f ngrok || true

        # Activate the virtual environment (if needed)
        source venv/bin/activate || exit 1

        # Start Uvicorn (FastAPI server) in the background
        echo "Starting FastAPI with Uvicorn..."
        nohup uvicorn main:app --host 0.0.0.0 --port 8001 > fastapi.log 2>&1 &

        # Give Uvicorn some time to start
        sleep   5

        # Check if Uvicorn started successfully
        echo "Checking Uvicorn status..."
        tail -n 20 fastapi.log

        # Start Ngrok to expose FastAPI (if Uvicorn is running)
        echo "Starting Ngrok..."
        which ngrok
        ngrok --version
        nohup ngrok http 8001 > ngrok.log 2>&1 &

        # Give Ngrok some time to start
        sleep 15

        # Check Ngrok logs for any errors
        echo "Checking Ngrok status..."
        tail -n 20 ngrok.log

        # Output final log files to check for errors or issues
        echo "Final FastAPI and Ngrok Logs:"
        tail -n 50 fastapi.log
        tail -n 50 ngrok.log
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
