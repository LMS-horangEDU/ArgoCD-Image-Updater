# Kubernetes를 활용한 3-Tier 환경 구축

## 스파르타 해커톤 (2023 항해커톤) 최우수상 수상

![해커톤 상장](https://github.com/LMS-horangEDU/Frontend/assets/83018778/e70130e3-e567-4ece-8c57-7c73b36b04de)

감사하게도 훌륭한 팀원들 덕분에 이번 해커톤에서 최우수 작으로 선정되어 우승하게 되었습니다.

## 시연

[시연 영상](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/6c7cb402-5bf4-46cc-90e6-bff46e885644)
<br/>

- 총 프로젝트 기간 9월 18일 ~ 9월 24일 (1주)

## CI/CD Workflow

![CICD](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/d419e4cd-2bfb-4a7d-ad3c-7f031b24914e)

#### Commit & Push -> Github Actions -> Image Build -> Registry -> ArgoCD Deploy

```
- 깃허브에서 코드가 Push되면 Github Actions를 사용하여 도커 이미지를 빌드합니다.
- 빌드된 이미지는 NCR(컨테이너 레지스트리)에 업로드됩니다.
- Yaml파일이 Push되면 argoCD가 업데이트된 정보를 쿠버네티스에 배포합니다.
```

## Architecture

![아키텍처](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/f378c28a-ca56-437c-806f-47c95c9685cb)

#### 구성도

```
- 네이버 쿠버네티스 서비스(NKS)를 이용하여 Bastion Controlplane 노드에
  쿠버네티스 CLI와 Helm을 설치하여 클러스터를 관리합니다.
- HPA(Horizontal Pod Autoscaler) 및 CA(Cluster Autoscaler)를 적용하여 스케일러블한 서버 환경을 구성하였습니다.
- 네이버 쿠버네티스 CSI를 활용하여 몽고DB에 블록 스토리지를 마운트하여 데이터의 안정성과 성능을 확보했습니다.
```

## Pod Configuration Diagram

![파드구성도](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/fe21db34-b40c-43fc-aa50-b831bdc2e2f1)

#### 내부 구성도

```
- 서비스는 Frontend, Backend, Database로 구성된 3-Tier 아키텍처로 구성되어 있습니다.
- Ingress를 통해 외부 Client가 Frontend UI에 접근하며, 서비스 간에는 클러스터 내부 IP를 통한 통신이 이루어집니다.
```

## Auto Scaling

![img](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/328a1915-a08e-44ac-ac3a-b96b0cf30e1b)

#### 오토스케일링

```
- Horizontal Pod Autoscaler는 컨테이너의 자원 사용률을 모니터링하여
컨테이너 수를 자동으로 조절합니다.
- Cluster Autoscaler는 노드의 자원 사용률을 모니터링하여 노드 수를 조절합니다.
- 프론트엔드 파드가 하나인 상태에서 부하를 증가시키자 새로운 파드가 생성되며
노드 리소스가 부족하여 팬딩 상태가 된 파드를 Cluster Autoscaler가
노드를 자동으로 확장하여 팬딩된 파드를 새로운 노드에 할당합니다.
```

<img width="957" alt="스크린샷 2023-12-16 201435" src="https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/b3a66112-26b0-465d-b23c-5eaa17762e9b">
<img width="929" alt="스크린샷 2023-12-16 201643" src="https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/64658d64-bfe0-486d-8aca-92f8429df426">

![2](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/3fde418d-4b34-4c9d-ae3c-0a2de7cc992e)
![3](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/26c88772-120b-4b7e-ab54-e692b90809d9)
![4](https://github.com/LMS-horangEDU/ArgoCD-Image-Updater/assets/88238006/78d7e077-baa3-4edf-9d72-0d6ec89c87f4)

## 느낀점

쿠버네티스에 어플리케이션을 배포하고 오토스케일링과 CSI 드라이버로 DB를 StatefulSet으로 구성하는 경험이 재미있었습니다. 또한 Helm으로 ArgoCD를 설치하여 배포를 자동화하는 경험을 할 수 있어 좋았습니다.
한편으로는 ArgoCD를 사용하면서 빌드된 이미지에 태그를 부여하는 것과 플러그인 관리에 대한 부분이 미흡했던 것 같습니다.
추후로는 차트뮤지엄을 통한 효율적인 차트 관리와 Service Mesh에 대한 이해를 바탕으로 Istio를 사용하여 트래픽을 관리하고 모니터링 하는 것이 목표입니다.

## Stacks | 사용 기술

**Architect**
<br/>
<br/>

<img src="https://img.shields.io/badge/Github-181717?style=for-the-badge&logo=GITHUB&logoColor=white"> <img src="https://img.shields.io/badge/GITHUB ACTIONS-007ACC?style=for-the-badge&logo=GITHUB ACTIONS&logoColor=white"> <img alt="Docker" src ="https://img.shields.io/badge/Docker-blue.svg?&style=for-the-badge&logo=Docker&logoColor=white"/> <img alt="Kubernetes" src ="https://img.shields.io/badge/Kubernetes-blue.svg?&style=for-the-badge&logo=Kubernetes&logoColor=white"/>

<img src ="https://img.shields.io/badge/NAVER CLOUD PLATFORM-00cc52?style=for-the-badge&logo=for-the-badge&logo=NAVER CLOUD PLATFORM&logoColor=white"/> <img src="https://img.shields.io/badge/ArgoCD-F24E1E?style=for-the-badge&logo=ARGOCD&logoColor=white"> <img src="https://img.shields.io/badge/VISUAL STUDIO CODE-007ACC?style=for-the-badge&logo=VISUAL STUDIO CODE&logoColor=white">

<br/>

<br/>

## 👨‍💻👩‍💻Who Made It | 서비스를 가꾼 사람들

| 이름   | 포지션           | Github or Email               |
| ------ | ---------------- | ----------------------------- |
| 심우진 | `Infra (Leader)` | https://github.com/woojinShim |
| 김연석 | `BackEnd`        | https://github.com/tastekim   |
| 이상원 | `FrontEnd`       | https://github.com/buddle6091 |
| 김진영 | `FrontEnd`       | https://github.com/wlsud801   |
