pipeline{
	agent any
	environment{
		image="real"
		version="latest"
		generatedImage="NA"
	}
	stages{
		stage('Initialize'){
			steps{
				echo """
					############
					# Starting #
					############
				"""
			}
		}
		stage('SCM checkout'){
			steps{
				git credentialsId: 'git', url: 'https://github.com/DuscraperRn/Test.git'
			}
		}
		stage('Image'){
			stages{
				stage('Build'){
					steps{
						script{
							def generatedImage=docker.build("duscraperrn/${image}:${BUILD_NUMBER}", "--no-cache .")
							env.gi=generatedImage
							docker.withRegistry('https://index.docker.io/v1/','dockercreds'){
								generatedImage.push()
							}
						}
					}
				}
				stage('Security check'){
					steps{
						script{
							//sh "trivy image duscraperrn/${image}:${version}"
							sh "trivy image duscraperrn/${image}:${version} -o /tmp/report-${image}-${version}-${BUILD_NUMBER}.txt"
						}
					}
				}
				stage('Push'){
					steps{
						script{
							//docker.withRegistry('https://index.docker.io/v1/','dockercreds'){
							//	env.gi.push()
							sh "kubectl create ns calculator${BUILD_NUMBER}"
							//}
						}
					}
				}
			}
		}
	}
}
