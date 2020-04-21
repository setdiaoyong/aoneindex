pipeline {
  agent {
    docker {
      image 'setzero/oneindex:v3.0'
    }
  }
  stages {
    stage('检出（源码库）') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: 'master']],
        userRemoteConfigs: [[url: env.GIT_REPO_URL, credentialsId: env.CREDENTIALS_ID]]])
      }
    }
    stage('构建并推到成品库') {
      steps {
        sh 'hugo version'
        sh 'export HUGO_ENV=production'
        sh 'hugo --minify --gc'
        sh 'tar -czf latest.tar.gz -C ./public .'
        sh 'curl -T latest.tar.gz -u 用户名:密码 https://用户名-generic.pkg.coding.net/源码库/composer/latest.tar.gz'
      }
    }
    stage('清除临时目录') {
      steps {
        sh 'rm -rf *'
        sh 'ls'
      }
    }
    stage('下载成品并发布（发布库）') {
      steps {
        sh 'rm -rf .git'
        sh 'curl -L -u 用户名:密码 \'https://用户名-generic.pkg.coding.net/源码库/composer/latest.tar.gz\' -o latest.tar.gz'
        sh 'ls'
        sh 'tar -xvf latest.tar.gz'
        sh 'ls'
        sh 'git init'
        sh 'git remote add origin https://e.coding.net/用户名/发布库.git'
        sh 'git add --all'
        sh 'git commit -m ${GIT_COMMIT} '
        sh 'git push -f https://发布库项目令牌（用户名）:发布库项目令牌（密码）@e.coding.net/用户名/发布库.git HEAD:master'
      }
    }
    stage('通知') {
      steps {
        sh 'echo '推送通知'
      }
    }
  }
}