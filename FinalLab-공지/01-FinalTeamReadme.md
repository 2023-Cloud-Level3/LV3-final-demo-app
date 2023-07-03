<style>
.burk {
    background-color: red;
    color: yellow;
    display:inline-block;
}
</style>

# 과제

Pre lab
Final Team
개인 과제

## Final Lab 조별 과제

Final Lab 조별 과제는 6월 12일(월)부터 시작


<Final Lab 관련 자료>
- 전체 현황 시트 링크: https://bit.ly/3WKrqMm
- Final Lab 과제 링크: https://bit.ly/3Ws6fOU
- 강사님 추가 보충 자료 링크: https://shorturl.at/bzNQ7 [Final Lab] 폴더

- 첨부: ’23년_Cloud Platform Eng. 전문가 과정_Final Lab 과제 수행 계획_0601(공유용)


담당자 : 
- 박지해 010-2656-2193  jhpark1@partner.sk.com
- 이수용 매니저 010-4042-0211  suyong.lee@sk.com



## 과제 설명

### 1. 과제개요
1) 과제명 : XXX 신규 서비스 론칭을 위한 클라우드 네이티브 인프라 구축
2) 개발기간 : 약 2개월 (9주)
3) 수행방식 : 팀 프로젝트(3~4명)

### 2. 추진배경 및 목적
1) 추진배경
   - XXX 서비스는 전세계 고객을 대상으로 2023년 10월 론칭을 목표로 준비 중인 라이브 커머스 서비스입니다. 10월 한국을 시작으로 12월 미국 론칭 예정입니다.
2) 과제목적
   - 고객 요청에 대한 빠른 대응을 위해, 인프라 배포와 **소스 코드 배포는 모두 자동화**   하고, 글로벌 확장성을 고려한 인프라 설계를 해야 합니다. 대규모 예산이 투입되는 만큼 특정 지역 이슈가 전체 서비스에 영향을 미치지 않도록 **글로벌 고가용성 인프라가 필수**적입니다.
### 3. 과제 범위
XXX 서비스 1차 서비스 론칭을 위한 클라우드 네이티브 아키텍처 설계 및 인프라 구축

1) 기능 요구사항 (Bold 체는 필수 항목)

   1. 일반

      - <span class=burk>퍼블릭클라우드</span> 기반아키텍처설계
      - 클라우드 서비스는기본적으로 IaaS 기반으로 제안하며CSP에서제공하는서비스를포함
      - 라이브커머스웹사이트에실시간스트리밍서비스구현필요
   2. 확장성/가용성

      - OS 및데이터에대한백업및복구
      - <span class=burk>리소스사용량에 따라서버의 자동증감</span> 지원(Auto Scale Up/Down, Out/In)
      - 서비스의 안정성을위한 리전 내 <span class=burk>HA(High Availability)</span> 구성
      - <span class=burk>부하분산</span> 기능을 제공(옵션,SSL Termination 구성)
      - 클라우드인프라자원에대한Naming rule계획수립및Tag설정
      - Network구성시IP소비계획(멀티클라우드통합IP관리방안제시)
      - 비용최적화고려한인프라구성수립할것

   3. Cloud Native Design

      - <span class=burk>컨테이너 사용시 CSP에서제공하는서비스로구성 </span>
      - VPC Network 플러그인은 클라우드 서비스에서 제공하는 기본플러그인을 사용한구성
      - 서비스확장에대한Scale-Out 구성
      - <span class=burk>컨테이너구성시 로그및데이터백업구성</span>
      - K8s 클러스터에접근하는사용자및서비스에대한권한부여와액세스제어
   4. Infrastructure as Code

      - 각CSP IaC 혹은 Terraform사용
      - <span class=burk>최대한의 환경을 IaC를 이용 배포 (기본설정, 쿠버네티스, 모니터링, 보안, 컨 테이너구성샘플 등)</span>
      - IaC 실행을 위한 구성 방안 (ex. IaC 코드를 안전하게 수행하기 위한 아키텍처 구조)
      - IaC코드관리방안(ex. Git을활용한코드관리및접근권한분리)
   5. Cloud  Security
      - 클라우드보안서비스들을활용한보안설계(예,WAF)
      - WEB/WAS/DB 3 Tier 구성을고려한네트워크설계
      - 사용자Access Control 설계
      - <span class=burk>Private/Public 네트워크분리 구성을통한 보안설계 </span>
      - 클라우드 인프라 계정의 보안 강화를 위하여 다양한 인증방식 활용(MFA 필     수)
      - <span class=burk>원격관리용서버 환경(Bastion 호스트구성 등) 제공</span>
      - 중요데이터가보관된스토리지에대한암호화
   
   6. Cloud Monitoring
      - <span class=burk>성능및장애모니터링(CPU, Memory, Disk, Network 등) 구성</span>
      - <span class=burk>접속기록을식별할수 있는형태로기록 및모니터링</span>
      - 클라우드포탈의모든작업에대한Audit 기능및자료보관제공
      - <span class=burk>알람구성 및Notification(이메일,메신저등)</span>
   7. 글로벌 확장성

      - 글로벌 고객을 위해 CSP 네트워크 서비스 최대한 활용하여 네트워크 지연일 최소화한인프라구축
      - 특정 지역 장애로 인한 서비스 장애를 회피하기 위한 <span class=burk>멀티 리전 구성 (Active- Active)</span>
   8. 개발지원

      - 오픈소스프로메테우스/그라파나를활용쿠버네티스모니터링
      - <span class=burk>신속한개발, 배포, 테스트를할 수있도록 환경제공(CI/CD)</span>
      - 서비스테스트자동화
      - DevOps Toolchain(Git및Repository 구성등)을활용한배포환경제공
      - Zero-Down time Deployment (Blue/Green, canary)
   9. Hybrid Cloud
      - Public 클라우드의장점과On-Premises의보안을결합한설계  <br>
        (ex, 웹서비스는Public Cloud에,DB는On-Premises에. 단On-Premises를직접구축할수없기에별도네트워크를구성하고On-Premises로가정한설계)

   10. Multi Cloud

      - Multi Cloud 운영을고려한통합모니터링/로그관리방안
      - 멀티클라우드를고려한컨테이너이미지및배포방안


### 4. 과제 산출물
1. 결과 보고서
   - Architecture 설계문서
     - 아키텍처 설계 방향성 및 의도
     - 아키텍처 요소들에 대한 설명
     - Cloud Infra 레벨 Architecture 문서
     - 모니터링 및 로깅 시스템 Architecture
   - 서비스 1개월 운영 예상 비용
     - 각 서비스별 예상 사용량 및 단가 기준 견적서
     - CSP 가격 계산기 캡쳐 화면 포함
   - 고가용성 테스트 결과 리포트
     - 정상 운영시 서비스 화면 vs 특정 사이트 장애 시 Fail-Over 적용된 화면
   - 조별 역할 분담 내용

2. 인프라 구축 가능한 IaC 코드 제출
   - 코드 실행 동영상 또는 화면 캡쳐 포함
   - IaC (Terraform/Bicep/CloudFormation 등) 코드 업로드 된 리포지토리 URL
3. Optional 제출
   1. 쿠버네티스 환경 확인을 위한 샘플 코드
      - Microservice 형태의 Stateful, Stateless 어플리케이션들 배포 환경 구축 및 샘플 제공 (yaml)
      - 서비스 안정성을 위한 고가용성 구축 및 샘플 제공 (yaml)
   2. CI/CD 파이프라인
      - CI/CD 파이프라인 도식화 문서
      - 배포 영상 또는 화면 캡쳐본
      - 소스 코드 업로드 된 리포지토리 URL


### 5. Check Point

Check Point
- 일반 요구사항을 적절히 만족하였는가
- 확장성/가용성을 위한 구성이 적절히 되어있는가?
- Cloud Native Design 요구사항에 맞는 설계대로 구성되었는가?
- Infrastructure as Code 요구사항에 맞는 자동화코드가 작성 및 동작하는가?
- Cloud Security 요구사항에 적절한 보안 요소 구성들을 구현하였는가?
- Cloud Monitoring 요구사항에 맞는 모니터링 시스템을 구축하였는가?
- 글로벌 확장성 요구에 맞는 고가용성 아키텍처를 구축하였는가?
- DevOps 요건에 맞는 지속적이고 자동화된 개발환경을 구축하였는가?

Opt.
- 타 클라우드 혹은 기존 온프레미스 환경으로의 확장성을 고려하여 디자인되었는가?
- 온프레미스 연동을 위한 아키텍처가 고려되었는가?


### 6. 참고사항
1. CSP(Cloud Service Provider)는 AWS 또는 Azure를 사용
2. Final Lab 프로젝트 수행기간 중, 총 클라우드 비용은 조별 최대 120만원($900), 개별 최대 15만원($100) 이내로 한정