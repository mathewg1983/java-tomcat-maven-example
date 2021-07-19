def tomcatwebapp = "ubuntu@http://ec2-54-209-115-168.compute-1.amazonaws.com/opt/tomcat/latest/webapps/"
def tomcatbin = "ubuntu@http://ec2-54-209-115-168.compute-1.amazonaws.com/opt/tomcat/latest/bin/"

pipeline
{
        agent
        {
            label 'master'
        }
        tools
        {
            maven 'myMaven'
            jdk 'myJDK'
        }
    stages
    {
        stage ('Checkout the code for upload')
        {
            steps
            {
                git branch: 'master', url: 'https://github.com/mathewg1983/java-tomcat-maven-example.git', credentialsId:'c39303ae-a79e-4c81-b109-3b80b73c0f4b'
            }
        }
        stage ('Code Compile')
        {
                steps
            {
                sh '''
                mvn validate
                mvn compile
                '''
            }
        }
        stage ('JUNIT Test')
        {
            when { expression { return env.TEST.contains('YES') } }
            steps
            {
                sh '''
                mvn test
                '''
            }
        }
        stage ('Packaging')
        {
            steps
            {
                sh '''
                mvn package
                '''
            }
        }

        stage ('Deploying onto tomcat')
        {
            withCredentials([sshUserPrivateKey(credentialsId: '3ff179fd-81d7-44f9-9d8c-279ec0f0e991', keyFileVariable: '/target/java-tomcat-maven-example.war')])
               {
                stage('scp-f/b') {
                    sh "scp -i ${tomcatwebapp} ${keyfile}"
                    echo 'pushed the war file'
                }
               }
        }
    }
}
