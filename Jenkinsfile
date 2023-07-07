pipeline {
    agent any
    
    environment {
        AZURE_RESOURCE_GROUP = 'test'
        AZURE_WEBAPP_NAME = 'jenkins-testing123'
        AZURE_CREDENTIALS_ID = 'cf76e075-f034-4a27-98b8-2cbf70d11266'
    }
    
    stages {
        stage('Build') {
            steps {
                // Checkout source code from version control system
                checkout scm
                
                // Build the Java application using Maven
                bat 'mvn clean package'
            }
        }
        
        stage('Deploy') {
            steps {
                // Authenticate with Azure using the Azure CLI plugin and the configured service principal credentials
                withCredentials([azureServicePrincipal(credentialsId: "${AZURE_CREDENTIALS_ID}", variable: 'AZURE_CREDENTIALS')]) {
                    bat 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID'
                }
                
                // Deploy the application to Azure Web App using Azure CLI plugin
                bat 'az webapp deploy --name "${AZURE_WEBAPP_NAME}" --resource-group "${AZURE_RESOURCE_GROUP}" --type war --src-path target/java-hello-world.war'
            }
        }
    }
}
