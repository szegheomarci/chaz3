# DevOps Strategy

## Microservice CI/CD
The source code is kept in a git repository. Development is restricted to the main branch, no feature branches are used according to modern CI/CD philosophy.
Code reviews are performed in gerrit.
A **verification Jenkins job** is triggered for new pachsets. The verification performs relevant checks and unit testing if applicable.
A **drop Jenkins job** is triggered for gerrit submit (new commit merge). This performs the same verification and testing as in verification, as this new commit is
cherry picked to the origin HEAD. This is necessary as the origin HEAD might have progressed since the last verification of the patchset. The build number is added
to the service version, and this is used as the tag for the docker image. The generated docker images is strored in a private docker registry, as these image versions
are not yet considered as release grade. If all successful, the commit in the git repository is also tagged with this image version.
The **release Jenkins job** is triggered manually. It takes the tag marking the docker image version as a parameter, fetches the image from the private repository
and publishes it on Docker Hub.
This process ensures that the microservice remains in a releasable state, and release candidates can go through further checking.
One track development and CI/CD means that incomplete features must be developed with appropriate strategies (Feature Toggle, Dark Launching, Branching by Abstraction),
in stead of feature branches. But it comes with the benefits of no merge conflict nightmare and one and only truth.
