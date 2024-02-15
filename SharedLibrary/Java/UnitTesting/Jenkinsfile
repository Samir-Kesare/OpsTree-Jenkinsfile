// Define the function for the Checkout stage
def checkoutStage() {
    return {
        stage("Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/Parasharam-DevOps/salary-api.git'
            }
        }
    }
}

// Define the function for the Test stage
def testStage() {
    return {
        stage("Test") {
            steps {
                echo "Executing Java Unit Testing"
                sh 'mvn test'
            }
        }
    }
}
