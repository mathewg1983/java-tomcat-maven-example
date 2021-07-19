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
                git branch: 'main', url: 'https://github.com/mathewg1983/java-tomcat-maven-example.git', credentialsId:'gitHUb_Mattu'
            }
        }
        stage ('Parallel block')
      {
            stage ('Code Compile')
        {
                steps
            {
                    sh '''
                mvn compile
                '''
            }
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
