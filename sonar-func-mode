pipeline {
agent any
stages {
stage('Checkout SCM') {
steps {
checkoutSCM()
}
}

stage('Compiling and Running Test Cases') {
steps {
compileAndRunTests()
}
}

stage('Generating a Cucumber Reports') {
steps {
generateCucumberReports()
}
}

stage('Creating Package') {
steps {
createPackage()
}
}

stage('Adding Generate Report') {
steps {
addGenerateReport()
}
}

stage('Install SonarQube CLI') {
steps {
installSonarQubeCLI()
}
}

stage('Analyzing Code Quality') {
steps {
analyzeCodeQuality()
}
}
}
}

def checkoutSCM() {
checkout scm: [$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/hellokaton/java11-examples.git']]]
}

def compileAndRunTests() {
sh 'mvn clean'
sh 'mvn compile'
sh 'mvn test'
}

def generateCucumberReports() {
script {
sh 'mvn verify'
}
}

def createPackage() {
sh 'mvn package'
}

def addGenerateReport() {
sh 'mvn verify'
}

def installSonarQubeCLI() {
sh '''
wget -O sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
unzip -o -q sonar-scanner.zip
rm -rf /opt/sonar-scanner
sudo mv --force sonar-scanner-5.0.1.3006-linux /opt/sonar-scanner
sudo sh -c 'echo "#/bin/bash \nexport PATH=\\\"$PATH:/opt/sonar-scanner/bin\\\"" >/etc/profile.d/sonar-scanner.sh'
sudo chmod +x /opt/sonar-scanner/bin/sonar-scanner
. /etc/profile.d/sonar-scanner.sh
'''
}

def analyzeCodeQuality() {
sh '''
/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=owtest23_sample-java-sonar \
-Dsonar.organization=owtest23 \
-Dsonar.qualitygate.wait=true \
-Dsonar.qualitygate.timeout=300 \
-Dsonar.sources=src/main/java/ \
-Dsonar.java.binaries=target/classes \
-Dsonar.host.url=https://sonarcloud.io \
-Dsonar.login=65558d8b45ebd4758f3e8d49b8f3582f8707306
'''
}
