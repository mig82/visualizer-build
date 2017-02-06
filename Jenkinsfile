def KonyPluginsManager

//The names of the Kony applications -both the Visualizer and the Mobile Fabric apps.
String 	visualizerAppName, //As it appears in Visualizer -a.k.a. the appid.
	mobileFabricAppName //As it appears in the Mobile Fabric Console.

// The git repository that stores the source files for the Visualizer app.
String 	gitVisualizerAppRepo, //The git URL to the repository.
	gitVisualizerAppBranch //The branch, tag or selector that defines which version of the repo to build from.

String 	gitCredentialsId,
		mobileFabricCredentialsId

Boolean buildAndroidPhoneNative,
		buildAndroidTabletNative,
		buildIPhoneNative,
		buildIPadNative,
		buildWindowsNative

node{

	def jenkinsWorkspace
	
	stage('Validate inputs'){
		visualizerAppName = VISUALIZER_APP_NAME
		mobileFabricAppName = MOBILE_FABRIC_APP_NAME

		gitVisualizerAppRepo = GIT_VISUALIZER_APP_REPO
		gitVisualizerAppBranch = GIT_VISUALIZER_APP_BRANCH

		gitCredentialsId = GIT_CREDENTIALS
		mobileFabricCredentialsId = MOBILE_FABRIC_CREDENTIALS

		buildAndroidPhoneNative = BUILD_ANDROID_PHONE_NATIVE
		buildAndroidTabletNative = BUILD_ANDROID_TABLET_NATIVE
		buildIPhoneNative = BUILD_IPHONE_NATIVE
		buildIPadNative = BUILD_IPAD_NATIVE
		buildWindowsNative = BUILD_WINDOWS_NATIVE
	}
	
	stage('Load Groovy modules'){
		jenkinsWorkspace = pwd() 
		//KonyPluginsManager = load("${workspace}@script/KonyPluginsManager.groovy")
		echo "Done loading Groovy modules from ${jenkinsWorkspace}"
	}
	
	stage('Build Visualizer app'){
		parallel(
			'android-phone-native': {
				if(buildAndroidPhoneNative=='true'){
					echo('Requested target build: Android phone native')
					build(
						job: 'android-phone-native-build',
						propagate: false,
						wait: true,
						parameters: [
							[$class: "StringParameterValue", 	name: "VISUALIZER_APP_NAME", 		value: "${visualizerAppName}"],
							[$class: "StringParameterValue", 	name: "MOBILE_FABRIC_APP_NAME", 	value: "${mobileFabricAppName}"],
							[$class: "StringParameterValue", 	name: "GIT_VISUALIZER_APP_REPO", 	value: "${gitVisualizerAppRepo}"],
							[$class: "StringParameterValue", 	name: "GIT_VISUALIZER_APP_BRANCH", 	value: "${gitVisualizerAppBranch}"],
							[$class: "PasswordParameterValue", 	name: "GIT_CREDENTIALS", 			value: "${gitCredentialsId}"],
							[$class: "PasswordParameterValue", 	name: "MOBILE_FABRIC_CREDENTIALS", 	value: "${mobileFabricCredentialsId}"]
						]
					)
				}
				
			},
			'android-tablet-native': {
				if(buildAndroidTabletNative=='true'){
					echo('Requested target build: Android tablet native')
					node('android'){
						if(isUnix()){
							sh("hostname")	
						}
						else{
							bat("hostname")
						}
					}
				}
			},
			'iphone-native': {
				if(buildIPhoneNative=='true'){
					echo('Requested target build: iPhone native')
					build(job: 'iphone-native-build', propagate: false, wait: true)
					/*node('ios'){
						sh("hostname")
					}*/
				}
			},
			'ipad-native': {
				if(buildIPadNative=='true'){
					echo('Requested target build: iPad native')
					node('ios'){
						sh("hostname")
					}
				}
			},
			'windows-native': {
				if(buildWindowsNative=='true'){
					echo('Requested target build: Windows')
					node('windows'){
						bat("hostname")
					}
				}
			}
		)
	}
}
