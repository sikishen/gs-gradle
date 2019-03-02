node {
    def myGradleContainer = docker.image('gradle:jdk8-alpine')     //设置docker image名称为 "gradle:jdk8-alpine"
    myGradleContainer.pull()                                       //拉取镜像至Jenkins docker本地
    stage('prep') {
        checkout scm                                               //将https://github.com/sikishen/gs-gradle/ 目录同步进Jenkins环境中
    }
    stage('test') {
        myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {   //将Docker Container中目录/home/gradle/.gradle映射至本地(dependentices)以便start不需要再次下载
            sh 'apk add --no-cache bash'
            sh 'cd complete && ./gradlew test'                     //cd进complete目录, 执行gradlew test命令
        }
    }
    stage('start') {
        myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {   //使用Test时下载的依赖
            sh 'apk add --no-cache bash'
            sh 'cd complete && ./gradlew start'                   //cd进complete目录, 执行gradlew start命令
        }
    }
}
