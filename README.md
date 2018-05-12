# AWS Seminar of AUSG for Like-Lion

AUSG의 AWS 세미나에 오신것을 환영합니다. 오늘 저희는 AWS와 Node.js를 활용하여 간단한 웹 서비스를 제작하는 실습을 진행할 것입니다.

### 본 실습 세션의 학습 목표는 아래와 같습니다.

- AWS Cloud9를 이용해 가상 환경을 띄워본다.
- AWS RDS(Relational Database Service)를 이용해 대표적인 RDBMS인 MySQL를 띄워본다.
- AWS S3(Simple Storage Service)를 이용해 스토리지를 구성해본다.
- 구성된 자원들을 이용해 웹앱을 개발해본다.

#  안내

## 0. 기본 사항
본 세션은 코딩 과정이 포함되어 있습니다. 모바일 환경(iPhone, iPad, Android)에서는 진행이 불가능하니 PC/Mac 환경에서 진행해 주세요.

## 1. AWS 계정
- AWS 계정 만들기 [이동](https://aws.amazon.com/ko/)

본 가이드는 한명이 하나의 AWS 계정을 사용한다고 가정합니다. AWS EC2, Cloud9, S3, RDS에 접근할 수 있어야 하며, 다른 사람과 계정을 공유하게되면 특정 리소스에 대해 충돌이 발생 할 수 있으므로 권장하지 않습니다.

본 실습의 일환으로 시작하는 모든 리소스는 AWS 계정이 12개월 미만인 경우, 제공하는 AWS 프리티어로 충분히 가능합니다. 프리티어를 넘어서는 경우, 과금 될 수도 있습니다. 따라서, 프리티어 기간이 지나셨다면, 새로운 실습용 계정을 만드시길 권장합니다. 자세한 내용은 [AWS 프리 티어 페이지](https://aws.amazon.com/free/)를 참조하세요.

## 2. 웹 브라우저
- Chrome 최신 버전 [다운로드](https://www.google.com/chrome/)
- Firefox 최신 버전 [다운로드](https://www.mozilla.org/ko/firefox/new/)

둘 중 원하시는 브라우저를 설치해주세요. (Internet Explorer는 AWS Web Console에서 문제가 발생 할 수 있습니다.)

##  순서대로 진행해 주세요.
1. [AWS Cloud9](1_C9/)
2. [AWS S3](2_S3/)
3. [AWS RDS](3_RDS/)
4. [Sequelize](4_Sequelize_js)
5. [SPA](5_SPA)
6. [삭제 가이드](6_Delete)
