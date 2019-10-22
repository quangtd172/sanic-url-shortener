pipeline {
    agent {
        label "agent-test"
    }
    environment {
        PROJECT_NAME        = "Sanic_url"
        PROJECT_URL         = "https://github.com/locvx1234/sanic-url-shortener.git"
        PROJECT_CREDENTIAL  =  ""
        
        SONAR_HOME = "${tool 'sonar-scanner-3'}" 
        SONAR_PROPERTIES = """\
        -Dsonar.host.url=https://sonar.learn.akawork.io \
        -Dsonar.login=d05973b70d5d6a9a62b7afaee0bdc9264477abe9 \
        -Dsonar.projectKey=${PROJECT_NAME} \
        -Dsonar.projectName=${PROJECT_NAME} \
        -Dsonar.sources='.' \
        -Dsonar.language='py' \
        -Dsonar.sourceEncoding='UTF-8' \
        -Dsonar.projectVersion='1.0' """
        
        // -Dsonar.login=242bb76619947c56271a1348b785e8452fdc2d08 \
        PATH="${env.SONAR_HOME}/bin:${env.PATH}"
    }
    
    stages {
        stage ("Checkout") {
            steps {
                checkout([$class: 'GitSCM', 
                            branches: [[name: '*']], 
                            doGenerateSubmoduleConfigurations: false, 
                            extensions: [], submoduleCfg: [], 
                            userRemoteConfigs: [[refspec: '+refs/heads/*:refs/remotes/origin/*', 
                                                    url: "${PROJECT_URL}", name: "origin"]]])
            }

        }
        
        stage ("Sonar Scan") {
            steps {
		sh "echo helloFromJenkinsfile"
                withSonarQubeEnv('DevOps-Sonar'){
                    sh "$SONAR_HOME/bin/sonar-scanner ${SONAR_PROPERTIES}"
                }
                // sh 'tox -e flake8'
            }
        }
        
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
            
            
        }
    }
    
    // post { 
    //     // always { 
    //     //     cleanWs()
    //     // }
    //     success {
    //         emailext (attachLog: false,
    //         body: 'Please check it out, link : $BUILD_URL',
    //         subject: "SUCCESS :Job ${env.JOB_NAME} - Build# ${env.BUILD_NUMBER}" ,
    //         to:'locvx1234@gmail.com')// Keep this mail, User can add personal email, separate with  ";"
    //     }
    //     failure {
    //         emailext (attachLog: true,
    //         body: 'Please check it out , link : $BUILD_URL',
    //         subject: "FAILED :Job ${env.JOB_NAME} - Build# ${env.BUILD_NUMBER}",
    //         to:'locvx1234@gmail.com')// Keep this mail, User can add personal email, separate with  ";"
    //     }
    // }
}
