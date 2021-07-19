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
    }
    post
      {
        always
          {
            junit 'target/surefire-reports/**/*.xml'
          }
      }
}
