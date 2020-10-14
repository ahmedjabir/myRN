def Checkout = 'Checkout'
def Build = 'Build'
def Archive = 'Archive'
def Deploy = 'Deploy'
def Publish = 'Publish'


pipeline {
    agent any
    tools {nodejs "nodejs"}
    stages {       
	    stage(CheckOut) {
	        steps {
		        checkout scm
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = CheckOut }
                }
            }
	    }
	    stage(Build) {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                echo '====== Build Started ======'
                sh 'npm install'
                sh 'npx pod-install'
		        sh 'xcodebuild clean -workspace ios/AndroidExp.xcworkspace -sdk iphoneos -scheme AndroidExp build -destination "platform=iOS Simulator,name=iPhone 8,OS=13.3"'
		        echo '====== Build Ended ======'
            }
        	post {
                failure {
                    script { env.FAILURE_STAGE = Build }
                }
            }
	    }
	    stage(Archive) {
	        steps {
	   	        echo '===== Test Started ====='
		        sh 'xcodebuild -workspace ios/AndroidExp.xcworkspace -scheme AndroidExp archive -sdk iphoneos -archivePath Files/markkit.xcarchive archive'
		        echo '===== Test Ended ====='
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = Archive }
                }
            }
	    }
	    
	    stage(Deploy) {
	        steps {
	   	        echo '===== Deployment Started ====='
		        sh 'xcodebuild -exportArchive -archivePath Files/markkit.xcarchive -exportPath Files -exportOptionsPlist exportOptions.plist -allowProvisioningUpdates'
		        echo '===== Deployment Compelted ====='
	        }
	        post {
                failure {
                    script { env.FAILURE_STAGE = Deploy }
                }
            }
	    }
     
     
     stage(Publish) {
         steps {
            echo '===== Publish Started ====='
             
                     
                     
            appCenter apiToken: '30d8938de76409402011c7a9b5dd47bd68e113b0',
                        appName: 'Markkit-QA',
                        buildVersion: '',
                        distributionGroups: 'Maqta-Testers',
                        notifyTesters: true,
                        ownerName: 'maqta.gateway.mobile',
                        pathToApp: 'Files/*.ipa',
                        pathToDebugSymbols: '',
                        pathToReleaseNotes: '',
                        releaseNotes: ''
                     
            echo '===== Publish Ended ====='
         }
         post {
             failure {
                 script { env.FAILURE_STAGE = Publish }
             }
         }
     }
    }
    
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
