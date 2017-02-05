def KonyPluginsXmlParser

def buildAndroidPhoneNative,
	buildAndroidTabletNative,
	buildIPhoneNative,
	buildIPadNative,
	buildWindowsNative

node{
	
	stage('Validate inputs'){
		buildAndroidPhoneNative = BUILD_ANDROID_PHONE_NATIVE
		buildAndroidTabletNative = BUILD_ANDROID_TABLET_NATIVE
		buildIPhoneNative = BUILD_IPHONE_NATIVE
		buildIPadNative = BUILD_IPAD_NATIVE
		buildWindowsNative = BUILD_WINDOWS_NATIVE
	}
	
	stage('Load Groovy modules'){
		def workspace = pwd() 
		//KonyPluginsXmlParser = load("${workspace}@script/KonyPluginsXmlParser.groovy")
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
							[$class: "StringParameterValue", 	name: "GIT_VISUALIZER_APP_REPO", 	value: "${GIT_VISUALIZER_APP_REPO}"],
							[$class: "StringParameterValue", 	name: "GIT_VISUALIZER_APP_BRANCH", 	value: "${GIT_VISUALIZER_APP_BRANCH}"],
							[$class: "PasswordParameterValue", 	name: "GIT_CREDENTIALS", 			value: "${GIT_CREDENTIALS}"],
							[$class: "StringParameterValue", 	name: "MOBILE_FABRIC_APP_NAME", 	value: "${MOBILE_FABRIC_APP_NAME}"],
							[$class: "PasswordParameterValue", 	name: "MOBILE_FABRIC_CREDENTIALS", 	value: "${GIT_CREDENTIALS}"]
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
