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
                powershell '''
                    dotnet restore WebAPIs.sln --source https://api.nuget.org/v3/index.json 
                    dotnet build WebAPIs.sln -p:Configuration=release -v:n
                '''
            }
        }
        stage('Test') {
            when {
                expression { params.REQUESTED_ACTION == 'TEST' }
            }
            steps {
                powershell '''dotnet test WebApiTests/WebApiTests.csproj'''
            }
        }
           stage('Build Solution') {
            when {
                expression { params.REQUESTED_ACTION == 'BUILD' }
            }
            steps {
                powershell '''dotnet publish WebAPIs.sln'''
            }
        }
       
    }
}
