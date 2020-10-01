pipeline {
  agent any
  stages {
    stage('Pull') {
      steps {
        git(url: 'https://github.com/ssrksiva/testpipeline6.git', branch: 'master', poll: true)
      }
    }

    stage('Check Java') {
      steps {
        sh 'java -version'
      }
    }

    stage('Clean') {
      steps {
        sh 'chmod +x mvnw'
        sh './mvnw -ntp clean -P-webpack'
      }
    }

    stage('install tools') {
      steps {
        sh './mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm -DnodeVersion=v10.16.0 -DnpmVersion=6.9.0'
      }
    }

    stage('npm install') {
      steps {
        sh './mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm'
      }
    }

    stage('backend test') {
      steps {
        sh './mvnw -ntp verify -P-webpack'
      }
    }

    stage('front end') {
      steps {
        sh './mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm -Dfrontend.npm.arguments=\'run test\''
      }
    }

    stage('packaging') {
      steps {
        sh './mvnw -ntp verify -P-webpack -Pprod -DskipTests'
        archiveArtifacts(fingerprint: true, artifacts: '**/target/*.jar')
      }
    }

    stage('Sonar') {
      steps {
        withSonarQubeEnv('My SonarQube Server') {
          sh './mvnw -ntp sonar:sonar'
        }

      }
    }

    stage('Deliver for development') {
      when {
        branch 'develop'
      }
      steps {
        sh 'git config --global user.name "ssrksiva"'
        sh 'git config --global user.email "sssrkbsc@gmail.com"'
        sh 'git tag -a tagName3 -m "test-admin3"'
        sh 'git commit -am "Merged develop branch to release"'
        sh 'git merge -s ours develop --allow-unrelated-histories'
		 withCredentials(bindings: [usernamePassword(credentialsId: 'testgithubcred', usernameVariable: 'user', passwordVariable: 'pass')]) {
          script {
            env.encodedPass=URLEncoder.encode(pass, "UTF-8")
          }
          sh 'git push https://${user}:${encodedPass}@github.com/${user}/testpipeline6.git HEAD:release -f'
        }
      }
    }
	 stage('Deliver for feature') {
      when {
        branch 'feature*'
      }
      steps {
        sh 'git config --global user.name "ssrksiva"'
        sh 'git config --global user.email "sssrkbsc@gmail.com"'
        sh 'git tag -a tagName3 -m "test-admin3"'
        sh 'git commit -am "Merged feature branch to develop"'
        sh 'git merge -s ours feature* --allow-unrelated-histories'
		 withCredentials(bindings: [usernamePassword(credentialsId: 'testgithubcred', usernameVariable: 'user', passwordVariable: 'pass')]) {
          script {
            env.encodedPass=URLEncoder.encode(pass, "UTF-8")
          }
          sh 'git push https://${user}:${encodedPass}@github.com/${user}/testpipeline6.git HEAD:develop -f'
        }
      }
    }
	stage('Deliver for hotfix') {
      when {
        branch 'hotfix*'
      }
      steps {
        sh 'git config --global user.name "ssrksiva"'
        sh 'git config --global user.email "sssrkbsc@gmail.com"'
        sh 'git tag -a tagName3 -m "test-admin3"'
        sh 'git commit -am "Merged hotfix branch to master"'
        sh 'git merge -s ours hotfix* --allow-unrelated-histories'
		 withCredentials(bindings: [usernamePassword(credentialsId: 'testgithubcred', usernameVariable: 'user', passwordVariable: 'pass')]) {
          script {
            env.encodedPass=URLEncoder.encode(pass, "UTF-8")
          }
          sh 'git push https://${user}:${encodedPass}@github.com/${user}/testpipeline6.git HEAD:master -f'
        }
      }
    }
	stage('Deliver for bugfix') {
      when {
        branch 'bugfix*'
      }
      steps {
        sh 'git config --global user.name "ssrksiva"'
        sh 'git config --global user.email "sssrkbsc@gmail.com"'
        sh 'git tag -a tagName3 -m "test-admin3"'
        sh 'git commit -am "Merged bugfix branch to develop"'
        sh 'git merge -s ours bugfix* --allow-unrelated-histories'
		 withCredentials(bindings: [usernamePassword(credentialsId: 'testgithubcred', usernameVariable: 'user', passwordVariable: 'pass')]) {
          script {
            env.encodedPass=URLEncoder.encode(pass, "UTF-8")
          }
          sh 'git push https://${user}:${encodedPass}@github.com/${user}/testpipeline6.git HEAD:develop -f'
        }
      }
    }
	stage('Deliver for release') {
      when {
        branch 'release*'
      }
      steps {
        sh 'git config --global user.name "ssrksiva"'
        sh 'git config --global user.email "sssrkbsc@gmail.com"'
        sh 'git tag -a tagName3 -m "test-admin3"'
        sh 'git commit -am "Merged release branch to master"'
        sh 'git merge -s ours release* --allow-unrelated-histories'
		 withCredentials(bindings: [usernamePassword(credentialsId: 'testgithubcred', usernameVariable: 'user', passwordVariable: 'pass')]) {
          script {
            env.encodedPass=URLEncoder.encode(pass, "UTF-8")
          }
          sh 'git push https://${user}:${encodedPass}@github.com/${user}/testpipeline6.git HEAD:master -f'
        }
      }
    }

  }
}