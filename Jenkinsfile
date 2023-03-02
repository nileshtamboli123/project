pipeline{
	agent{
		node{
			label "built-in"
		}
	}
	stages{
		stage("clone") {
			steps{
				sh "yum install docker -y"
				sh "systemctl start docker"
				sh "rm -rf /mnt/nilesh"
				dir("/mnt/nilesh/23qa1") {
					sh "git clone https://github.com/nileshtamboli123/project.git -b 23qa1"
					//sh "chmod 777 /mnt/nilesh/23qa1/project/index.html"
                    sh "aws s3 cp /mnt/nilesh/23qa1/project s3://nilesh11/project1 --recursive"
				}
				dir("/mnt/nilesh/23qa2") {
					sh "git clone https://github.com/nileshtamboli123/project.git -b 23qa2"
					//sh "chmod 777 /mnt/nilesh/23qa2/project/index.html"
                    sh "aws s3 cp /mnt/nilesh/23qa2/project s3://nilesh11/project2 --recursive"
				}
				
			}
        }

				stage("slave-1") {
                    agent{
                        node {
                            label "slave-1"
                        }
                    }
                    
					steps {
						  sh "sudo docker stop 23qa11"
						  sh "sudo docker rm 23qa11"
					      sh "sudo docker run -itdp 80:80 --name 23qa11 httpd"
                          sh "aws s3 cp s3://nilesh11/project1 /mnt/project1 --recursive"
					      sh "sudo docker cp /mnt/project1/index.html 23qa11:/usr/local/apache2/htdocs/" 
					}

				}
				stage("slave-2") { 
                     agent{
                        node {
                            label "slave-2"
                        }
                    }
                     
					steps{
						sh "sudo docker stop 23qa22"
						sh "sudo docker rm 23qa22"
					    sh "sudo docker run -itdp 90:80 --name 23qa22 httpd"
                        sh "aws s3 cp s3://nilesh11/project2 /mnt/project2 --recursive"
					    sh "sudo docker cp /mnt/project2/index.html 23qa22:/usr/local/apache2/htdocs/" 
					}
				}
	}
}
