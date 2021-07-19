def tomcatwebapp = 'ubuntu@http://ec2-54-209-115-168.compute-1.amazonaws.com:/opt/tomcat/latest/webapps/'
def tomcatbin = 'ubuntu@http://ec2-54-209-115-168.compute-1.amazonaws.com:/opt/tomcat/latest/bin/'
def tomcatwebapplc = '/opt/tomcat/latest/webapps/'
def tomcatbinlc = '/opt/tomcat/latest/bin/'
def targetfile = 'java-tomcat-maven-example/target/java-tomcat-maven-example.war'

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
            steps
            {
                sshagent(credentials: ['3ff179fd-81d7-44f9-9d8c-279ec0f0e991'])
                {
                    sh "scp -o StrictHostKeyChecking=no ${targetfile} ${tomcatwebapp}"
                    println($tomcatwebapplc)
                    echo 'War Deployed to target server'

                }
            }
        }
    }
}
