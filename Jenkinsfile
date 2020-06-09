node {
    stage('Trigger Bitbucket Pipeline') {
        withCredentials([string(credentialsId: 'bitbucket-msl-parser-connection-string', variable: 'endpoint')]) {
            def scmVar = checkout scm
            def gitBranch = scmVar.GIT_BRANCH
            
            println ${scmVar}

            if (gitBranch ==~ /.*master/ || gitBranch ==~ /.*develop/) {
                def bitbucketBranch = gitBranch ==~ /.*master/ ? "master" : "develop"
                def requestBody = """
                {
                    "target": {
                        "ref_type": "branch", 
                        "type": "pipeline_ref_target", 
                        "ref_name": "${bitbucketBranch}"
                    }
                }
                """
                retry (3) {
                    def response = httpRequest authentication: "bitbucket-msl-api-credentials", acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', httpMode: 'POST', requestBody: requestBody, url: endpoint
                    println(response)
                }
            } else {
                echo "Ignore branch ${env.BRANCH_NAME}"
            }
        }
    }
}
