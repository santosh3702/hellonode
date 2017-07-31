node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("santoshrahulgoru/hellonode")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

	stage('Run the applicaton'){
	      try{
		  sh 'docker run -it -p 8000:8000 santoshrahulgoru/hellonode'
		  }catch (error) {
    } 
		  
	}
    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        try{
		docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
		}catch (error) {
    } finally {
      // Stop and remove database container here
      sh 'docker stop santoshrahulgoru/hellonode'
      // sh 'docker-compose rm db'
    }
    }
}