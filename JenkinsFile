pipeline 
{
        agent 
        {
            label 'master'
        }
        tools 
        {
            maven 'mymaven'
            jdk 'myJDK'
        }
    stages 
    
    {

        stage ('Checkout the code') 
        {
            steps
            {
                git branch: 'main', url: 'https://github.com/mathewg1983/java-tomcat-maven-example'
            }
        }

      stage ('Parallel block') 
      {
        parallel 
                {   
                    stage ('Code Validate') 
                    {
                        steps  
                        {
                                    sh """
                                        mvn validate
                                    """
            
                                
                            
                        }
                    }
                
      

        stage ('Code Compile') 
        {
            steps
            {
               
                sh """
                mvn compile
                """
            
            }
        }
        }
      } 
      

        stage ('JUNIT Test') 
        {
            when {expression {return env.TEST.contains('YES') }}
            steps
            {
                sh """
                mvn test
                """
            }
        }

        stage ('Packaging') 
        {
            steps 
            {
                sh """
                mvn package
                """

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