pipeline {
    agent any
    parameters {
        choice(
            choices: ['BUILD' , 'DEPLOY'],
            description: '',
            name: 'REQUESTED_ACTION')
        string(
              name: 'GIT_URL',
              defaultValue: 'https://github.com/tavisca-mgandhe/WebAPIs.git',
              description: 'repository path')
        string(
               name: 'WEB_API_SOLUTION_FILE',
               defaultValue: 'WebAPIs.sln',
               description: 'solution path')
        string(
             name: 'TEST_PROJECT_PATH',
             defaultValue: 'WebApiTests/WebApiTests.csproj',
             description: 'test path')
        string(
            name: 'DOCKER_USERNAME', 
            defaultValue: 'mayankgandhe',
             description: 'docker username')
        string(
            name: 'DOCKER_PASSWORD',
            defaultValue:'',
            description: 'docker password')
        string(
            name: 'DOCKER_REPO_NAME',
            defaultValue:'mayankgandhe/webapi',
            description: 'docker repository')
        string(
            name: 'IMAGE_VERSION',
            defaultValue:'latest',
            description: 'docker image version')
        string(
            name: 'SOLUTION_NAME', 
            defaultValue: 'WebApi',
            description: 'solution name')
    }
    stages {
        stage('Build') {
            when {
                expression { params.REQUESTED_ACTION == 'BUILD' || params.REQUESTED_ACTION == 'DEPLOY' }
            }
            steps {
                
              
                 bat "  dotnet build ${WEB_API_SOLUTION_FILE} -p:Configuration=release -v:n"
               
                 bat " dotnet test ${TEST_PROJECT_PATH}"
               
                 bat " dotnet publish ${WEB_API_SOLUTION_FILE} -c Release -o ../publish"
                
                 powershell " compress-archive WebApis/bin/Release/netcoreapp2.2/publish/ artifact.zip -Update"
                
                 bat "docker build -t %DOCKER_REPO_NAME%:%IMAGE_VERSION% --build-arg project_name=%SOLUTION_NAME%.dll ."
                
            }
        }
         stage('Deploy') {
             when {
                expression { params.REQUESTED_ACTION == 'DEPLOY' }
            }
            steps {
                
                echo "----------------------------Deploy-----------------------------"
                bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
               bat" docker push ${DOCKER_REPO_NAME}:${IMAGE_VERSION}"
                            deleteDir()

            }
        }
        
    }
}

