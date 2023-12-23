# ChaZ3 App DevOps Strategy

## Overview
ChaZ3 is a cloud native application, assembled from a set of microservices. Microservices are implemented as containers and defined using Helm charts. Each service is individually built, tested, and released.


## Microservice CI/CD
The microservice source code is kept in a git repository. **Trunk based development** is followed, meaning there are no feature branches, only the main branch.
Code reviews are performed in gerrit.

Jenkins is used as build automation tool. Jenkins jobs are defined as code, stored in the git repository.

A **verification Jenkins job** is triggered by new pachsets.  
The verification performs relevant checks and unit testing if applicable.  
A docker image is also built, and is tagged with the unique <software_version>-verif<build_number>, e.g. `myservice:1.2.5-verif152`. This is however cleaned up after the build.

A **drop Jenkins job** is triggered by gerrit submit.  
This job performs the same verification and testing as in verification, as this new commit is
cherry picked to the origin HEAD. This is necessary as the origin HEAD might have progressed since the last verification of the patchset.  
The docker image tag is made up from the service version and a sequence number that increases with every commit by one since the last release, e.g. `myservice:1.2.5-4`. The generated docker image is strored in the github packages docker registry, as these image versions
are not yet considered as release grade. If the build and storage are successful, the commit in the git repository is also tagged with this image version.

The **release Jenkins job** is triggered manually.  
It takes the tag marking the docker image version as a parameter, fetches the image from the github packages docker registry
and publishes it on Docker Hub.
A new commit is generated that increases the patch version number of the service and also adds tags the git repository.

This process ensures that the microservice remains in a releasable state, and release candidates can go through further checking.
One track development and CI/CD means that incomplete features must be developed with appropriate strategies (Feature Toggle, Dark Launching, Branching by Abstraction),
in stead of feature branches. But it comes with the benefits of no merge conflict nightmare and one and only one truth.

## Tools used in ChaZ3 App  

Containerization:  
 - <img src="https://landscape.cncf.io/logos/docker-member.svg" height="30" /> Docker

**App Definition and Development**  
Database: 
 - <img src="https://landscape.cncf.io/logos/mongo-db.svg" height="30" /> MongoDB
 - <img src="https://landscape.cncf.io/logos/my-sql.svg" height="30" /> MySQL  

Streaming & Messaging:
 - <img src="https://landscape.cncf.io/logos/kafka.svg" height="30" /> Kafka

Application Definition & Image Build:
 - <img src="https://landscape.cncf.io/logos/helm.svg" height="30" /> Helm

Continuous Integration & Delivery:
 - <img src="https://upload.wikimedia.org/wikipedia/commons/4/4d/Gerrit_icon.svg" height="30" /> Gerrit
 - <img src="https://landscape.cncf.io/logos/jenkins.svg" height="30" /> Jenkins
 <!-- - <img src="https://landscape.cncf.io/logos/git-hub-actions.svg" height="30" /> GitHub Actions -->

**Orchestration & Management**  
Scheduling & Orchestration:
* <img src="https://landscape.cncf.io/logos/kubernetes.svg" height="30" /> Kubernetes

**Provisioning**  
Automation & Configuration:
* <img src="https://landscape.cncf.io/logos/terraform.svg" height="30" /> Terraform
* <img src="https://landscape.cncf.io/logos/ansible.svg" height="30" /> Ansible
