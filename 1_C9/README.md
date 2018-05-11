# AWS Cloud9
![c9](https://i.imgur.com/rzZMKYN.png)

AWS Cloud9은 인터넷만 연결되어 있다면 웹 브라우저상으로 코드 작성 및 실행, 디버깅을 할 수 있는 클라우드 기반의 통합 개발 환경(IDE)를 의미합니다.
**Ctrl + 왼쪽마우스 클릭!**
**https://aws.amazon.com/ko/cloud9/**
<br>

## AWS Cloud9 생성하기 
* 싱가폴 리전 선택
![스크린샷, 2018-01-10 20-35-15](https://i.imgur.com/C4v5zVW.png)

* AWS Cloud9 시작하기 버튼 --> 클릭
![스크린샷, 2018-01-10 20-38-12](https://i.imgur.com/jDNs9SR.png)
* 지역은 싱가폴로 선택을 하도록 하겠습니다.
![스크린샷, 2018-01-10 20-42-17](https://i.imgur.com/G1HBFzt.png)
* Create Environment 버튼 --> 클릭
* Create a new instance for environment (EC2 설정) --> Instance Type은 t2.micro설정
![스크린샷, 2018-01-10 20-49-26](https://i.imgur.com/5ivNdsk.png)
* Cost-saving setting은 4시간 후 설정
* Create! 하면 조금 시간이 걸립니다...
    * 이때, 자동으로 EC2가 생성됩니다.

## EC2 Elastic IP (고정아이피 할당)
* [Ctrl + 마우스 왼쪽 버튼 클릭!](https://aws.amazon.com/ko/)
* 내계정 -> AWS Management Console-> EC2
* 좌측 Instance탭 -> 생성한 C9의 EC2 인스턴스 선택(aws-cloud9-[C9_이름] 으로 시작합니다.) -> 하단의 Private IP 확인
![Instance](images/Instance.png)
* NETWORK & SECURITY탭 -> 탄력적 IP -> 새 주소 할당 -> 할당하기 -> 할당된 IP클릭 -> 작업-> 주소연결클릭
![Instance](images/elasticIP_1.png)
* 인스턴스 -> 생성한 C9의 EC2 인스턴스 선택
* 프라이빗 IP -> 생성한 C9의 EC2 인스턴스의 Private IP 선택
![Instance](images/elasticIP_2.png)

## EC2 Inbound 열기
* [Ctrl + 마우스 왼쪽 버튼 클릭!](https://aws.amazon.com/ko/)
* 콘솔에 접근  -> EC2 -> NETWORK & SECURITY탭
* Security Groups
* Inbound -> Edit  -> 아래 사진과 같이 추가
![inbound](https://i.imgur.com/MLrtqy2.png)
![스크린샷, 2018-01-10 21-30-51](images/Inbound.png)

실습이 완료되면 다음모듈인 [2. S3 생성하기](../2_S3) 으로 이동하십시오
