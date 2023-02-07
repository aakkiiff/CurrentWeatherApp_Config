pipeline {
    agent any
    environment {
        
    
        GIT_REPO = "https://github.com/aakkiiff/CurrentWeatherApp_Config/" 

        AUTH_APP_NAME = "current_weather_app_auth"
        UI_APP_NAME = "current_weather_app_ui"
        WEATHER_APP_NAME = "current_weather_app_weather"

        // AUTH_APP_IMAGE = "${DOCKERHUB_USERNAME}/${AUTH_APP_NAME}"
        // UI_APP_IMAGE = "${DOCKERHUB_USERNAME}/${UI_APP_NAME}"
        // WEATHER_APP_IMAGE = "${DOCKERHUB_USERNAME}/${WEATHER_APP_NAME}"
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

        stage("UPDATE K8S DEPLOYMENT FILES"){
            steps{
                echo "${IMAGE_TAG}"
                sh "sed -i 's/${AUTH_APP_NAME}.*/${AUTH_APP_NAME}:${IMAGE_TAG}/g' ./k8s/auth-deployment.yml"
                sh "sed -i 's/${UI_APP_NAME}.*/${UI_APP_NAME}:${IMAGE_TAG}/g' ./k8s/ui-deployment.yml"
                sh "sed -i 's/${WEATHER_APP_NAME}.*/${WEATHER_APP_NAME}:${IMAGE_TAG}/g' ./k8s/weather-deployment.yml"

            }
        }
    }
}