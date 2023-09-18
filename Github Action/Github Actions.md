# Github Actions 코어 개념 

- `Workflow`: 자동화된 전체 프로세스를 나타낸 순서도 <br>
→ 하나 이상의 Job으로 구성, Event에 의해 예약되거나 트리거 될 수 있는 자동화 절차 <br>
→ Github에게 YAML파일로 정의한 자동화 동작을 전달하면, Github Actions는 해당 파일을 기반으로 그대로 실행

- `Event`: workflow 실행 기준<br> 
→ cron과 같이 시간에 따라 작업을 실행하게 할 수도, git push / pull-request 등의 GIthub Repository 이벤트를 기준으로 실행하게 할 수도 있다

- `Job`: 그룹의 역할<br>
→ 여러 Step으로 구성, 단일 가상 환경 제공 <br>
→ 다른 Job에 의존 관계를 가질 수도 있고, 독립적으로 병렬로 실행될 수도 있음 

- `Step`: Job안에서 순차적으로 실행되는 프로세스 단위 <br>
→ 파일 시스템을 통해 서로 정보를 공유, 교환 <br>
→ step에서 명령을 내리거나, action을 실행

- `Action`: 타인들 또는 작성자에 의해서 미리 정의된 호출 매커니즘을 불러와 사용 <br>
→ **job을 구성하기 위한 step 들의 조합으로 구성된 독립적인 명령**, workflow의 가장 작은 빌드 단위 (action이 step을 포함해야 한다) <br>
→ 사용자가 직접 커스터마이징하거나, 어느 나/그룹/개발팀/제품/회사 등에서 정의한 action을 불러와 사용할 수 있음 

- `Runner`: Gitbub Action Runner 어플리케이션이 설치된 머신<br>
→ Workflow가 실행될 인스턴스 


# Workflow 생성 

: yml 파일로 각 Action workflow를 설정

> 레파지토리에 등록한 프로젝트 폴더의 최상위에 `.github/workflow/*.yml` 로 위치

## Workflow 설정

[예시 `workflow.yml` 파일]
```yml
name: Node CI
on: [push]
jobs: ## job 들을 명시
  build: ## job id
    runs-on: ubuntu-latest ## 해당 job의 구동 환경을 정의
    strategy:
      matrix:
        node-version: [8.x, 10.x, 12.x]
    steps:
      - uses: actions/checkout@v1
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      - name: Npm Install, build and test
        run: |
          npm ci
          npm run build --if-present
          npm test
        env:
          CI: true
```

- `최상위 name`: 해당 Workflow 이름 명시 

- `on`: 해당 Workflow의 Event 명시 
    - `cron` 문법으로 시간 설정 
    ```yml
    # 10분마다 실행 workflow
    on:
        schedule:
            - cron: '*/10 * * * *'
    ```
    - `push|pull-request` 등의 깃헙 이벤트를 구독
    ```yml
    # master, dev 브랜치에 push 된 경우에 실행
    on:
        push:
            branches: [master, dev]
    ```
    - 실행할 브랜치 제한 
    ```yml
    # master, dev 브랜치에 push 되었고,
    # js파일의 변경이 있을 때에만 트리거,
    # doc 디렉토리의 변경에는 트리거 하지 않게 하고싶은 경우
    on:
    push:
        branches: [master, dev]
            paths:
                - "**.js"
            paths-ignore:
            - "doc/**"
    ```

## `job`에 대한 설정 
- `jobs`: jobs에 등록된 job들은 기본적으로 병렬적으로 실행
    - `job_id`를 키값으로 하여 생성할 수 있음
    ```yml
    jobs:
        some-job-id: ...
        some-job-id-2: ...
    ```
    - [jobs 예시 yml 파일]
    ```yml
    jobs:
        build:
            strategy:
                matrix:
                    node-version: [10.x, 12.x]

            runs-on: ubuntu-latest
            steps:
                - name: Checkout source code ## 소스코드를 checkout 하는 step
                  uses: actions/checkout@v2

                - name: Cache yarn dependencies ## yarn dependencies를 cache 하는 step
                  uses: actions/cache@v1
                  id: yarn-cache
                  with:
                    path: node_modules
                    key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
                    restore-keys: |
                        ${{ runner.os }}-yarn-
    ```
    - ✅ `jobs.<job_id>.runs-on **Required**`: 해당 job을 실행할 컴퓨팅 자원(runner)을 명시 (어떤 OS에서 실행할 것인지 명시)

    - ✅ `jobs.<job_id>.strategy`: strategy는 jobs가 여러 환경에서의 테스트/배포 등을 위해 build matrix를 설정
        -  다른 환경(다른 Nodejs 버전) 들을 명시해 여러 환경에서의 같은 jobs를 동시에 실행

    - ✅ `jobs.<job_id>.steps`: **job이 가질 수 있는 순차적인 동작 나열**
        - 명령어 실행
        - setup
        - 깃헙 코드 checkout
        - github marketplace에 있는 action 가져와서 실행
        - 도커 이미지로 생성
        - 도커 이미지 배포
        - aws/gcp 등에 서비스 배포하는 작업 등 
    - 각각의 step은 job의 컴퓨팅 자원에서 <u>독립적인 프로세스</u>로 동작
    - job의 runner(컴퓨팅 자원)의 파일 시스템에 접근

        - `jobs.<job_id>.steps.name`: step의 이름 명시
          - github actions 페이지에서 workflow구동 로그를 확인할 때 보여짐

        - `jobs.<job_id>.steps.uses`: <u>해당 스텝에서 사용할 action</u> 선택
            - github 에서는 action을 사용 시, version을 명시하여 사용하기를 강력히 추천
            - `{owner}/{repo}@{ref|version}`
            - 각 action 는 필수적으로 document가 필요

            - [run 예시 yml 파일]
            ```yml
            jobs:
                some-job:
                    steps:
                    - name: My First Step
                      run: | ## 명령어를 여러 줄 사용하기 위해서는 다음과 같이 한다.
                          npm install
                          npm test
                          npm build
            ``` 
            - `jobs.<job_id>.steps.run`: job에 할당된 컴퓨팅자원의 shell을 이용하여 command line program을 구동
    - ✅ `jobs.<job_id>.env`: 해당 job의 컴퓨팅 자원에 설정 할 환경변수를 key=value의 형태로 명시

    - [예시 yml]
    ```yml
    jobs:
        build:
            strategy:
                matrix:
                    node-version: [10.x, 12.x]

            runs-on: ubuntu-latest
            steps:
                - name: Checkout source code 
                  uses: actions/checkout@v2
                
                - name: My First Step
                  run:
                    npm install
                    npm test
                    npm build

                - name: Cache yarn dependencies 
                  uses: actions/cache@v1
                  id: yarn-cache
                  with:
                    path: node_modules
                    key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
                    restore-keys: |
                        ${{ runner.os }}-yarn-
    ```
    - ✅ `jobs.<job_id>.with`: 해당 action에 의해 정의되는 input 파라미터 <br>
    → key/value 페어로 되어있다 <br>
    → input 파라미터는 환경 변수로 설정 <br>
    → `INPUT_`이라는 prefix가 붙음 <br>
