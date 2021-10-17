node ("worker"){
    stage("NodeJS setup") {
        env.NODEJS_HOME="${tool 'nodejs'}"
        env.PATH="${env.NODEJS_HOME}/bin:${env.PATH}"
        sh "echo `npm --version`"
        sh "rm results.tar.gz"
}
    stage("Checkout") {
        git url: "https://github.com/Tellz777/mdt"
}
    stage("Compressing") {
            parallel (
                "Compress-js": {
                    sh "ls www/js/ | xargs -I{i} uglifyjs www/js/{i} -o www/min/{i} -c"

},
                "Compress-css": {
                    sh "ls www/css/ | xargs -I{i} cleancss www/css/{i} -o www/min/{i}"
        }
    )  
}
    stage("Archiving results") {
        sh "tar --exclude='www/js' --exclude='www/css' --exclude='.git' -zcvf results.tar.gz *"
        archiveArtifacts artifacts: 'results.tar.gz', onlyIfSuccessful: true
    }
}
