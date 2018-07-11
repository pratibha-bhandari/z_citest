node {

    //Wipe the workspace so that we are building completely clean
    //deleteDir()

    stage('Checkout') {

        // Checkout files.
        checkout([
            $class: 'GitSCM',
            branches: [[name: '*/develop']],
            doGenerateSubmoduleConfigurations: false,
            extensions: [[$class: 'CleanBeforeCheckout']], submoduleCfg: [],
            userRemoteConfigs: [[
                credentialsId: '91877f3d-b2fd-42a8-92b2-5c37d8b5f42c',
                url: 'http://Parijatchandra@scmci.noncd.rz.db.de/bitbucket/scm/zcit/z_citest.git'
            ]]
        ])
    }

    stage('build') {
        // Build
        //sh 'security unlock-keychain -p jenkins ${HOME}/Library/Keychains/login.keychain'

        sh 'security set-key-partition-list -S apple-tool:,apple:,codesign: -s -k niit@123 login.keychain'

        sh '/usr/bin/xcodebuild -scheme JenkinsTesting -configuration Release clean build archive -archivePath /Users/Shared/Jenkins/Home/workspace/z_citest/build/Release-iphoneos/JenkinsTesting.xcarchive DEVELOPMENT_TEAM=UAWU67869T'

        sh '/usr/bin/xcodebuild -exportArchive -archivePath /Users/Shared/Jenkins/Home/workspace/z_citest/build/Release-iphoneos/JenkinsTesting.xcarchive -exportPath /Users/Shared/Jenkins/Home/workspace/z_citest/build -exportOptionsPlist /Users/Shared/Jenkins/Home/workspace/z_citest/build/developmentUAWU67869TExport.plist'
    }

    stage('fastlane') {
        sh 'whereis fastlane'

        dir ('/Users/Shared/Jenkins/PipeLineTestProject') {
            fastlane("test")
        }
       //sh 'fastlane("beta")'
    }

    stage('test') {
        //testing
        //sh 'xcodebuild -scheme "JenkinsTesting" -configuration "Debug" build test -destination "platform=iOS Simulator,name=iPhone 8,OS=11.2" -enableCodeCoverage YES | /usr/local/bin/xcpretty -r junit'
    }

    stage('post-build') {
        // Publish test restults.
        //step([$class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: 'build/reports/junit.xml'])
    }

    stage('archive') {
        //Archiving artifacts
        archiveArtifacts '**'
    }
}

def fastlane(lane) {
def env = [
"PATH+LOCAL=/usr/local/bin/",
"http_proxy=http://dmzproxy.tech.rz.db.de:8080",
"https_proxy=http://dmzproxy.tech.rz.db.de:8080",
"LC_ALL=en_US.UTF-8",
"LANG=en_US.UTF-8",
"FASTLANE_EXPLICIT_OPEN_SIMULATOR=2"
]

withEnv(env) {
timeout(time: 30, unit: 'MINUTES') {
wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'XTerm', 'defaultFg': 1, 'defaultBg': 2]) {
sh "fastlane ${lane}"
}
}
}
}



