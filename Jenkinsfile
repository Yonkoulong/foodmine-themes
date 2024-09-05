pipeline {
    agent any
    
    environment {
        NPM_TOKEN = credentials('npm_token')  // Store NPM_TOKEN securely in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git branch: 'main', url: 'https://github.com/Yonkoulong/foodmine-themes.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // If you have a build process (e.g., SCSS compilation)
                sh 'npm run build'
            }
        }

        stage('Bump Version') {
            steps {
                // Automatically bump the version (patch by default)
                sh 'npm version patch -m "Auto-bump to version %s [ci skip]"'
            }
        }

        stage('Publish to GitHub Packages') {
            steps {
                sh 'npm publish --access public --registry=https://npm.pkg.github.com/'
            }
        }

        stage('Commit and Push Version Bump') {
            steps {
                // Commit and push the updated version number
                sh 'git config --global user.email "you@example.com"'
                sh 'git config --global user.name "Your Name"'
                sh 'git add package.json'
                sh 'git commit -m "Bump version [ci skip]"'
                sh 'git push origin main'
            }
        }
    }
    
    post {
        success {
            echo 'Package published successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}