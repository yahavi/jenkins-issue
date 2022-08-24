pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'gradle-resolve', url: "https://github.com/yahavi/project-examples.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtGradleDeployer (
                    id: "GRADLE_DEPLOYER",
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
                    rootDir: "gradle-examples/gradle-example-ci-server/",
                    tasks: 'clean artifactoryPublish',
                    deployerId: "GRADLE_DEPLOYER",
                    resolverId: "GRADLE_RESOLVER"
                )
            }
        }
    }
}