# Set up a Docker Gitlab instance 

1. Start a Gitlab CE instance
 ``` 
 https://gitlab.com/elvadas.nono/greeting/
 ```




```
helm repo add gitlab https://charts.gitlab.io/
helm repo update
$ helm search repo gitlab/gitlab
NAME                 	CHART VERSION	APP VERSION	DESCRIPTION
gitlab/gitlab        	3.1.1        	12.8.1     	Web-based Git-repository manager with wiki and ...
gitlab/gitlab-omnibus	0.1.37       	           	GitLab Omnibus all-in-one bundle
gitlab/gitlab-runner 	0.14.0       	12.8.0     	GitLab Runner
```



Add a gitlab-ci.yml file to your repository 

```
gitlab_runner:
  image: docker:latest
  stage: build
  variables:
    DOCKER_HOST: tcp://localhost:2375
    DOCKER_TLS_CERTDIR: ""

  services:
    - docker:18.09-dind
  tags:
    - kubernetes
  before_script:
    - docker info
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
  script:
    - docker build --pull -t "$CI_REGISTRY_IMAGE:$CI_COMMIT_REF_SLUG" .
    - docker push "$CI_REGISTRY_IMAGE:$CI_COMMIT_REF_SLUG"
```



2. Setup a Gitlab Runner in kubernetes

```
helm repo add gitlab https://charts.gitlab.io

helm install gitlab-runner -f values-runner.yaml  gitlab/gitlab-runner
```

