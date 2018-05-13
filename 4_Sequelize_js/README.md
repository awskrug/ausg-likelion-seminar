# Sequelize.js
이번 챕터에서는 RDBMS ORM인 Sequelize.js를 RDS에 연결해봅니다.  
  
## 설치
우리의 프로젝트에 Sequlize를 설치합니다.
아까 만든 Cloud9으로 이동해 다음 명령어를 터미널에 입력해주세요.

```bash
$ npm install mysql2 sequelize --save
```

설치가 완료되었습니다. 이제 코드를 수정해볼까요?

## Sequelize 연동
index.js 코드를 다음과 같이 수정합니다.

```javascript
const express = require('express')
const bodyParser = require('body-parser')
const cors = require('cors')
const app = express()

///////// 여기서부터
const Sequelize = require('sequelize')
const sequelize = new Sequelize('DB이름', '마스터사용자이름', '마스터암호', {
    host: '확인한엔드포인트주소',
    dialect: 'mysql',
});
///////// 여기까지 추가되었습니다.

app.use(cors())
app.use(bodyParser.json())
app.get('/', function (req, res) {
    res.json({
        message: "Hello, Like-Lion! We're AUSG!",
    })
})

app.listen(3000)
```

## 모델 정의
Sequelize를 이용해 Model을 새롭게 정의해줍니다.  
  
```javascript
const express = require('express')
const bodyParser = require('body-parser')
const cors = require('cors')
const app = express()

const Sequelize = require('sequelize')
const sequelize = new Sequelize('DB이름', '마스터사용자이름', '마스터암호', {
    host: '확인한엔드포인트주소',
    dialect: 'mysql',
});

///////// 여기서부터
const ImagePost = sequelize.define('image_post', {
    id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true,
    },
    username: Sequelize.STRING,
    title: Sequelize.STRING,
    description: Sequelize.TEXT,
    url: Sequelize.STRING,
})
ImagePost.sync({ alter: true })
///////// 여기까지 추가

app.use(cors())
app.use(bodyParser.json())
app.get('/', function (req, res) {
    res.json({
        message: "Hello, Like-Lion! We're AUSG!",
    })
})

app.listen(3000)
```

## 엔드포인트 만들기
몇가지 엔드포인트를 만들어보겠습니다.

- GET /images
> 현재 DB에 업로드된 자료를 모두 가져옵니다.

- POST /images
> 현재 DB에 새 자료를 업로드합니다.
  
코드를 수정합니다.

```javascript
const express = require('express')
const bodyParser = require('body-parser')
const cors = require('cors')
const app = express()

const Sequelize = require('sequelize')
const sequelize = new Sequelize('DB이름', '마스터사용자이름', '마스터암호', {
    host: '확인한엔드포인트주소',
    dialect: 'mysql',
});

const ImagePost = sequelize.define('image_post', {
    id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true,
    },
    username: Sequelize.STRING,
    title: Sequelize.STRING,
    description: Sequelize.TEXT,
    url: Sequelize.STRING,
})

ImagePost.sync({ alter: true })

app.use(cors())
app.use(bodyParser.json())

app.get('/', function (req, res) {
    res.json({
        message: "Hello, Like-Lion! We're AUSG!",
    })
})

///////// 여기서부터
app.get('/images', function (req, res) {
    ImagePost.findAll()
        .then(function (result) {
            res.json(result)
        })
        .catch(function (error) {
            res.json(error)
        })
})

app.post('/images', function (req, res) {
    ImagePost.create({
        title: req.body.title,
        description: req.body.description,
        url: req.body.url,
    }).then(function () {
        res.json({
            message: 'success!',
        })
    }).catch(function (error) {
        res.json({
            message: 'failed!',
        })
        console.log(error)
    })
})
///////// 여기까지 추가

app.listen(3000)
```

## S3 업로드용 Pre-signed URL을 만드는 엔드포인트 추가

우리의 프로젝트에 AWS-SDK를 설치합니다.
다음 명령어를 터미널에 입력해주세요.

```bash
$ npm install aws-sdk --save
```

엔드포인트를 추가합니다.  
  
  ```javascript
const express = require('express')
const bodyParser = require('body-parser')
const cors = require('cors')
const app = express()

const Sequelize = require('sequelize')
const sequelize = new Sequelize('DB이름', '마스터사용자이름', '마스터암호', {
    host: '확인한엔드포인트주소',
    dialect: 'mysql',
});

const ImagePost = sequelize.define('image_post', {
    id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true,
    },
    username: Sequelize.STRING,
    title: Sequelize.STRING,
    description: Sequelize.TEXT,
    url: Sequelize.STRING,
})

ImagePost.sync({ alter: true })

app.use(cors())
app.use(bodyParser.json())

app.get('/', function (req, res) {
    res.json({
        message: "Hello, Like-Lion! We're AUSG!",
    })
})

app.get('/images', function (req, res) {
    ImagePost.findAll()
        .then(function (result) {
            res.json(result)
        })
        .catch(function (error) {
            res.json(error)
        })
})

app.post('/images', function (req, res) {
    ImagePost.create({
        title: req.body.title,
        description: req.body.description,
        url: req.body.url,
    }).then(function () {
        res.json({
            message: 'success!',
        })
    }).catch(function (error) {
        res.json({
            message: 'failed!',
        })
        console.log(error)
    })
})  

///////// 여기서부터
const AWS = require('aws-sdk')
const s3 = new AWS.S3({
    region: 'ap-southeast-1',
    signatureVersion: 'v4',
})

app.post('/generatePresignedUrl', function (req, res) {
    const key = `${Date.now()}_${req.body.filename}`
    const params = {
        Bucket: '버킷 이름',
        Key: key,
        ACL: 'public-read',
    }
    const presignedUrl = s3.getSignedUrl('putObject', params)
    res.json({
        url: key,
        presignedUrl,
    })
})
///////// 여기까지 추가

app.listen(3000)
```

짠! 최종 코드가 완성되었습니다. Postman을 활용해서 각 엔드포인트 별 결과를 확인해보세요!
  
축하드립니다! 실습이 완료되셨으면, 다음 모듈인 [SPA 웹앱 올리기](../5_SPA)로 이동하세요.
