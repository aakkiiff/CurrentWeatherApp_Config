pipeline {
    agent any
    environment {

        GIT_REPO = "https://github.com/aakkiiff/CurrentWeatherApp_Config/" 
        AUTH_APP_NAME = "current_weather_app_auth"
        UI_APP_NAME = "current_weather_app_ui"
        WEATHER_APP_NAME = "current_weather_app_weather"
    }

    stages{

        stage('CLEANUP WORKSPACE'){
            steps{
                script{
                    cleanWs()
                }
            }
        }

        stage("CHECKOUT GIT REPO"){
            steps{
                git "${GIT_REPO}"
            }
        }

        stage("UPDATE K8S DEPLOYMENT FILES AND PUSH TO THE GIT REPO"){
            steps{

                sh 'cat ./k8s/auth-deployment.yml'
                sh "sed -i 's/${AUTH_APP_NAME}.*/${AUTH_APP_NAME}:${IMAGE_TAG}/g' ./k8s/auth-deployment.yml"
                sh "sed -i 's/${UI_APP_NAME}.*/${UI_APP_NAME}:${IMAGE_TAG}/g' ./k8s/ui-deployment.yml"
                sh "sed -i 's/${WEATHER_APP_NAME}.*/${WEATHER_APP_NAME}:${IMAGE_TAG}/g' ./k8s/weather-deployment.yml"
                sh 'cat ./k8s/auth-deployment.yml'

                sh 'git config --global user.email jackakif@gmail.com'
                sh 'git config --global user.name aakkiiff'
                sh 'git add ./k8s/'
                sh "git commit -m 'Updated deployment files to ${IMAGE_TAG}'"

                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'pass', usernameVariable: 'uname')]) {
                    sh 'git push https://$uname:$pass@github.com/aakkiiff/CurrentWeatherApp_Config.git master'
                }
            }
        }
    }
}