# Jenkins nodes 

## Jenkins as container 

### Local network 
docker network create -d bridge jenkins-network

### Master
docker run -d --network jenkins-network --name jenkins-master -p 8080:8080 -p 50000:500
00 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11 

## Agents (as containers)

1.- Create key pair  
 ssh-keygen -f ~/.ssh/jenkins_agent_key   
2.- Set credentials in Jenkins (master)  
¡Ojo! Comprobar la existencia de la variable JAVA_HOME en el nodo. Sino, pued incluirse como variable al configurar el nodo.
3.- Create agent (local)

docker run -d --rm --name=agent1 --network jenkins-network -p 22:22 \
-e "JENKINS_AGENT_SSH_PUBKEY=ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDQvFvYIev0SDlZtucVxWmFEXgusv7l1sQwHWEvbeLw0sTvE0ay0mP2sHY2HzsMN32X+9hhuUTxJICG3XU4aqzH0HlIUjPvJBM5IbLeiCkyfyZ1IrQsfA10u81uvoIBRJPeTMCblYS4h2aNpM8i5YxYyIr0B3bzW0udiLbe0t/1Q0gJK0FFvhBrnvFNTL76hJS23PoLqHd+rf125P37m227Lfq8vlYpm0ga6c4k9Dv3saAcY1SqjZSMNAF7n35hiMmnrPdRjwx+CMl6qohgIuypq0Q8RAS/mvsbOPM1opDJ236xZZCR5i8Y2Hn3uUh4U84fiCopcZbO4+R4DG1DMS1w/LTZbdi39qFmU1GQjuDhIZ59Y3A/NPDBi8cbVE+iv+p/VgzXJBap6tiMS81bznqZB0rsm1BhoHYQQkKLGD1Vdwkmzc/0q5ssVIIB0/MNO3DWwxTyCcVguZUYPDWCJOTVCgZGitIWpLrenxMIA8VqqSo2XUyJMcx8oazkPTS+yDs= amartin@AT-5CD11365GS" \
jenkins/ssh-agent:alpine

4.- Check that both containers belong to the saem network.

docker network inspect jenkins-network

https://www.jenkins.io/doc/book/using/using-agents/

## Agents (on premise ready docker pipelines)


1.- Remote node: RHEL 8
- Install Docker and Git.  
- Work and temp folder (java.io.tmp)  > 1GB
  - See Node monitoring option in Jenkins

2.- At Jenkins
- Intall the following plugins on Jenkins: Docker, Docker pipelines
- Config:
Jenkins /System configuration
Set Docker label at Declarative Pipeline (Docker) section.
All docker nodes requires this label in order to be picked by docker pipelines.
Also include docker registry.



