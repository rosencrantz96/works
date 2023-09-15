# CI/CD

## CI(Continuous Integration)
지속적 통합의 줄임말 

## CD(Continuous Delivery)
지속적 전달의 줄임말 

> 애플리케이션 개발단계를 자동화 시켜 애플리케이션을 보다 짧은주기로 고객에게 제공하는 방법

### CI/CD의 과정
- 소스코드 테스트
- 빌드
- 컨테이너화
- 통합적인 저장소에 전달 후 서비스 무 중단 배포 등 

CI - 테스트, 빌드, Dockerizing, 저장소에 전달까지 프로덕션 환경으로 서비스를 배포할 수 있도록 준비하는 프로세스 

CD - 저장소로 전달된 프로덕션 서비스를 실제 사용자들에게 배포하는 프로세스 

## CI/CD 툴
> CircleCI, TravisCI, **Jenkins**, Atlassian, Bamboo 등 

### Github Action
> github에서 공식적으로 제공하는 CI/CD 툴(**개발 워크 플로우 자동화** 툴)

개발 환경이 분산되지 않고 일원화 된 개발환경(github)을 통해 많은 모든 과정을 통합적으로 관리할 수 있다 

- npm에 패키지 배포
- Docker Hub에 이미지 배포
- AWS에 서비스 배포
- GCP에 서비스 배포
→ git/github 기능인 Pull Request, Push 등을 통해 github에서 바로 가능 

> 참고 링크
https://hwasurr.io/git-github/github-actions/
https://velog.io/@ggong/ 
<br>
Github-Action%EC%97%90-%EB%8C%80%ED%95%9C-%EC%86%8C%EA%B0%9C%EC%99%80-%EC%82%AC%EC%9A%A9%EB%B2%95
<br>
https://tech.kakaoenterprise.com/180