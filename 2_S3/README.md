# AWS S3

## S3 생성하기
- AWS Console로 이동
- S3 서비스로 이동
![스크린샷 1](./images/screenshot-2018-02-18-PM-10.17.26.png)
- '버킷 만들기' 클릭
![스크린샷 2](./images/screenshot-2018-02-18-PM-10.17.30.png)
- 팝업 창이 뜨면, '버킷 이름'에 **고유한 이름(중복불가)** 을 입력합니다.
* 싱가포르 리전 선택
![스크린샷 3](./images/3.png)
- 모든 설정을 기본 설정 값으로 두고 '다음' 클릭 (두 번)
* '버킷 만들기' 클릭
![스크린샷 4](./images/4.png)
- 만든 버킷을 클릭합니다
![스크린샷 5](./images/screenshot-2018-02-18-PM-10.19.22.png)
- '권한' 클릭  
- '버킷 정책' 클릭

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:DeleteObject",
        "s3:GetObject",
        "s3:PutObject"
        ],
      "Resource": "arn:aws:s3:::버킷이름입력/*"
    }
  ]
}
```

위의 값을 복사, 붙여넣기 해주신 후 버킷 생성시 입력한 도메인을 넣어주세요
![스크린샷 6](./images/screenshot-2018-02-18-PM-10.31.14.png)


실습이 완료되면 다음모듈인 [3. AWS RDS](../3_RDS) 으로 이동하십시오