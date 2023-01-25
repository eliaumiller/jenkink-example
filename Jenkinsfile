import jxl.*
import java.time.format.DateTimeFormatter
pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {                
                    bat 'mvn clean compile'                
            }
        }

        stage ('Testing Stage') {

            steps {                
                   bat 'mvn -Dmaven.test.failure.ignore test'         
            }
        }
		
		stage('WaitPeriod') {
    script {
	    def currentTime = new Date()
        String nextStageStartTimeFormat = currentTime.format("dd/MM/yyyy") + " 19:00"
        def nextStageStartTime = new SimpleDateFormat("dd/MM/yyyy HH:mm").parse(nextStageStartTimeFormat)
        def difference= nextStageStartTime.getTime() - currentTime.getTime()
        //Difference is in milliseconds
        if (difference < 0) {
            difference = 86400000 + difference // The Big number is 24 hours in milliseconds
        }
        println "Difference(milliseconds): " + difference
        sleep(time:"${difference}", unit: "MILLISECONDS")
    }
}


       
    }    
    post {         
         always {
             junit '**/target/surefire-reports/TEST-*.xml'
        }              
    }
}
