pipeline {
    agent any

    environment {
        // 🌿 Branch name (multibranch Jenkins compatible)
        BRANCH = "${env.GIT_BRANCH}".replace("origin/", "")
    }

    stages {

        // ============================
        // 🌍 ENVIRONMENT SELECTION
        // ============================
        stage('🌍 Select Environment') {
            steps {
                script {
                    if (BRANCH == 'main') {
                        echo "🚀 Production Deployment Selected"
                        env.SERVER_HOST = credentials('PROD_HOST')
                        env.SERVER_USER = credentials('PROD_USER')
                        env.SSH_KEY     = 'PROD_SSH_KEY'
                        env.DEPLOY_DIR  = '/var/www/html/laravel-prod-app'
                        env.GIT_BRANCH  = 'main'
                    } else {
                        echo "🧪 Staging Deployment Selected"
                        env.SERVER_HOST = credentials('STAGING_HOST')
                        env.SERVER_USER = credentials('STAGING_USER')
                        env.SSH_KEY     = 'STAGING_SSH_KEY'
                        env.DEPLOY_DIR  = '/var/www/html/laravel-staging-app'
                        env.GIT_BRANCH  = 'staging'
                    }
                }
            }
        }

        // ============================
        // 🚀 DEPLOY LARAVEL APP
        // ============================
        stage('🚀 Deploy Laravel Application') {
            steps {
                sshagent(credentials: [env.SSH_KEY]) {
                    sh """
                    set -e

                    echo "🖥️ Server: ${SERVER_USER}@${SERVER_HOST}"
                    echo "📁 Deploy Dir: ${DEPLOY_DIR}"
                    echo "🌿 Branch: ${GIT_BRANCH}"

                    ssh -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_HOST} << 'EOF'
                      set -e

                      cd ${DEPLOY_DIR} || {
                        echo "❌ Deploy directory not found"
                        exit 1
                      }

                      REPO_URL="https://${GH_USERNAME}:${GH_PAT}@github.com/your-org/laravel-app.git"

                      if [ -d ".git" ]; then
                        echo "🔄 Updating existing repository"
                        git remote set-url origin "\$REPO_URL"
                        git fetch origin
                        git checkout ${GIT_BRANCH} || git checkout -b ${GIT_BRANCH} origin/${GIT_BRANCH}
                        git reset --hard origin/${GIT_BRANCH}
                      else
                        echo "📥 Cloning repository"
                        git clone -b ${GIT_BRANCH} "\$REPO_URL" .
                      fi

                      echo "✅ Laravel deployment completed"
                    EOF
                    """
                }
            }
        }
    }

    // ============================
    // 🧹 CLEAN JENKINS WORKSPACE
    // ============================
    post {
        always {
            echo "🧹 Cleaning Jenkins workspace"
            cleanWs()
        }

        success {
            echo "✅ Jenkins pipeline completed successfully 🚀"
        }

        failure {
            echo "❌ Jenkins pipeline failed — check logs 🔥"
        }
    }
}
