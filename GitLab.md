# GitLab

<br><br>

## Pipeline
- `.gitlab-ci.yml` in project root (https://docs.gitlab.com/ee/ci/yaml/#cache)
- pipeline jobs are runned by gitlab runner
- jobs (with defined stage: prop) are runned in the order as defined in `stages: ...` in top of file
- to get data of one stage on to the next you can use `artifacts: ..` property

## Adding variables to your applications
Go to project repo and click on **Settings > CI/CD > Variables** 

## Cache
```yml
cache:
  key: ${CI_COMMIT_REF_SLUG}   # cache unique identifying key
  paths:
    - node_modules/            # what to cache
  policy: push                 # set the upload and download behavior of a cache (pull, push, pull-push)
```


## Sample .gitlab-ci.yml file
```yml
stages:
  - build
  - test

build website:
  stage: build
  image: node
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
  artifacts: 
    paths:
      - public

test artifact:
  image: alpine
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html

test website:
  image: node
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve &
    - sleep 3
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"

```
