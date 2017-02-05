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
					echo('Building Android phone native')
					//build(job: 'android-phone-build', propagate: false, wait: false)
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
			'android-tablet-native': {
				if(buildAndroidTabletNative=='true'){
					echo('Building Android tablet native')
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
					echo('Building iPhone native')
					//build(job: 'iphone-build', propagate: false, wait: false)
					node('ios'){
						sh("hostname")
					}
				}
			},
			'ipad-native': {
				if(buildIPadNative=='true'){
					echo('Building iPad native')
					node('ios'){
						sh("hostname")
					}
				}
			},
			'windows-native': {
				if(buildWindowsNative=='true'){
					echo('Building Windows')
					node('windows'){
						bat("hostname")
					}
				}
			}
		)
	}
}
