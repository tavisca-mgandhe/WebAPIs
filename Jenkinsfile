pipeline {
    agent any
    parameters {
        choice(
            choices: ['BUILD' , 'TEST'],
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
    }
    stages {
        stage('Build') {
            when {
                expression { params.REQUESTED_ACTION == 'BUILD' || params.REQUESTED_ACTION == 'TEST' }
            }
            steps {
                powershell "
                    dotnet restore ${WEB_API_SOLUTION_FILE} --source https://api.nuget.org/v3/index.json
                        dotnet build ${WEB_API_SOLUTION_FILE} -p:Configuration=release -v:n
                "
            }
        }
        stage('Test') {
            when {
                expression { params.REQUESTED_ACTION == 'TEST' }
            }
            steps {
                bat "dotnet test ${TEST_PROJECT_PATH}"
            }
        }
           stage('Publish') {
            when {
                expression { params.REQUESTED_ACTION == 'BUILD' }
            }
            steps {
                bat "dotnet publish ${WEB_API_SOLUTION_FILE}"
            }
        }
    }
}
