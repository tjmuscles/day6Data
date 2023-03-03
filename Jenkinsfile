node {
    stage('Checkout') {
        git url: 'https://github.com/tjmuscles/day6Data.git'
    }
    
    stage('Gradle build') {
        sh 'gradle clean build'
    }
    
    stage ("Gradle Bootjar-Package - DataService") {
        sh 'gradle bootjar'
    }
	
	stage ("Containerize the app-docker build - DataApi") {
        sh 'docker build --rm -t mcc-data:v1.0 .'
    }
    
    stage ("Inspect the docker image - DataApi"){
        sh "docker images mcc-data:v1.0"
        sh "docker inspect mcc-data:v1.0"
    }
    
    stage ("Run Docker container instance - DataApi"){
        sh "docker run -d --rm --name mcc-data -p 8080:8080 mcc-data:v1.0"
     }
    
    stage('User Acceptance Test') {
    
    	def response= input message: 'Is this build good to go?',
    	parameters: [choice(choices: 'Yes\nNo',
    	description: '', name: 'Pass')]
    	
    	if(response=="Yes") {
		    stage('Release- DataService') {
			 sh "docker stop mcc-data"
		     sh 'echo MCC DataService is ready to release!'
	
		    }
		}
	}
}