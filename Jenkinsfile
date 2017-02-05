def KonyPluginsManager

String 	visualizerAppName,
		mobileFabricAppName

String 	gitVisualizerAppRepo,
		gitVisualizerAppBranch

String 	gitCredentialsId,
		mobileFabricCredentialsId

Boolean buildAndroidPhoneNative,
		buildAndroidTabletNative,
		buildIPhoneNative,
		buildIPadNative,
		buildWindowsNative

node{
	
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
		def workspace = pwd() 
		//KonyPluginsManager = load("${workspace}@script/KonyPluginsManager.groovy")
		echo "Done loading Groovy modules"
	}
	
	stage('Build'){
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
					/*node('android'){
						if(isUnix()){
							sh("hostname")	
						}
						else{
							bat("hostname")
						}
					}*/
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
