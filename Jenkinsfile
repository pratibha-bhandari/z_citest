node {

    //Wipe the workspace so that we are building completely clean
    //deleteDir()

    stage('Checkout') {

        // Checkout files.
        checkout([
            $class: 'GitSCM',
            branches: [[name: '*/master']],
            doGenerateSubmoduleConfigurations: false,
            extensions: [[$class: 'CleanBeforeCheckout']], submoduleCfg: [],
            userRemoteConfigs: [[
                credentialsId: '63c69d9f-e5d0-4496-895f-2bc55da86ddb',
                url: 'https://github.com/pratibha-bhandari/z_citest.git'
            ]]
        ])
    }



stage('build') {

sh 'whereis fastlane'

dir ('/Users/Shared/Jenkins/Home/workspace/PipelineTest') {
    fastlane("build")
}
}

    /*stage('fastlane') {
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
    }*/
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



