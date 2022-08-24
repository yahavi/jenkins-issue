pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git url: "https://github.com/lorenyeung/jenkins-issue", branch: 'main'
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtGradleDeployer (
                    id: "GRADLE_DEPLOYER1",
                    serverId: "ECOSYS",
                    repo: "libs-release-local"
                )

                rtGradleResolver (
                    id: "GRADLE_RESOLVER",
                    serverId: "ECOSYS",
                    repo: "libs-release"
                )
            }
        }

        stage ('Exec Gradle') {
            steps {
                rtGradleRun (
                    tool: "gradle",
                    tasks: 'clean artifactoryPublish',
                    deployerId: "GRADLE_DEPLOYER",
                    useWrapper: true,
                    resolverId: "GRADLE_RESOLVER",
                    buildFile: "build.gradle"
                )
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
