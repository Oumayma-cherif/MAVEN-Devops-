 def success () {

    def imageUrl = 'https://semaphoreci.com/wp-content/uploads/2020/02/cic-cd-explained.jpg' // Replace with the actual URL of your image
    def imageWidth = '800px' // Set the desired width in pixels
    def imageHeight = 'auto' // Set 'auto' to maintain the aspect ratio


        
        echo "Sending success email..."
        emailext (  
                 body: """
                <html>
                <body>
                    <p>YEEEEY, The Jenkins job was successful.</p>
                    <p>You can view the build at: <a href="${BUILD_URL}">${BUILD_URL}</a></p>
                    <p><img src="${imageUrl}" alt="Your Image" width="${imageWidth}" height="${imageHeight}"></p>
                </body>
            </html>
        """,
            subject: "Jenkins Build - Success",
          to: 'oumayma.cherif@esprit.tn', // Updated email address
                 from: 'oumayma.cherif@esprit.tn', // Updated email address
                 replyTo: 'oumayma.cherif@esprit.tn', // Updated email address
                 mimeType: 'text/html' 
                  )
        echo "Success email sent."
    }
   def  failure () {

    def imageUrl = 'https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ytlj68SIRGvi9mecSDb52g.png' // Replace with the actual URL of your image
    def imageWidth = '800px' // Set the desired width in pixels
    def imageHeight = 'auto' // Set 'auto' to maintain the aspect ratio


        
        echo "Sending failure email..."
        emailext ( 
                body: """
                <html>
                <body>
                    <p> eewww , The Jenkins job Failed .</p>
                    <p>You can view the build at: <a href="${BUILD_URL}">${BUILD_URL}</a></p>
                    <p><img src="${imageUrl}" alt="Your Image" width="${imageWidth}" height="${imageHeight}"></p>
                </body>
            </html>
        """,
            subject: "Jenkins Build - Failure",
             to: 'oumayma.cherif@esprit.tn', // Updated email address
                 from: 'oumayma.cherif@esprit.tn', // Updated email address
                 replyTo: 'oumayma.cherif@esprit.tn', // Updated email address
                 mimeType: 'text/html'
                  ) 
        echo "Failure email sent."
    }

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean install'
                // List the contents of the target directory for debugging
                sh 'ls -la target'
            }
        }

        
         stage('Email Notification') {
            steps {
                script {
                    currentBuild.resultIsBetterOrEqualTo('SUCCESS') ? success() : failure()
                }
            }
        }
      
  }
    
}


