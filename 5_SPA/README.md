# SPA
Vue.js로 클라이언트 코드를 작성한 뒤, express를 이용해 호스팅합니다.  
  
## SPA 앱 붙여넣기
- Cloud9 환경에서 `app` 폴더에서 오른쪽 클릭해, `New Folder`를 선택해 `public`이라는 이름을 가진 폴더를 만듭니다
- 만든 폴더에서 오른쪽 클릭해, `New File`을 선택합니다.
- 새로운 파일명을 `index.html`로 설정합니다.
- `index.html`을 열고 아래의 코드를 그대로 복사, 붙여넣기 합니다.

```html
<!doctype html>
<html>
  <head>
    <title>like-lion-ausg</title>
    <meta charset="utf-8" />
  </head>
  <body>
    <div id="app">
      <h1>AUSG Instagram</h1>
      <div class="upload">
        <input id="f" type="file" accept="image/png, image/gif, image/jpeg, image/bmp, image/x-icon"/>
        <input id="t" type="text" v-model="title" placeholder="title"/>
        <input id="d"type="text" v-model="description" placeholder="description"/>
        <button @click="upload">업로드</button>
      </div>
      <div class="image-posts">
        <div
          class="image-posts__post"
          v-for="imagePost in imagePosts"
          :key="imagePost.id"
        >
          <img :src="imagePost.url" style="width: 100%"/>
          <h2>{{ imagePost.title }}</h2>
          <p>{{ imagePost.description }}</p>
        </div>
      </div>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.16/vue.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.18.0/axios.min.js"></script>
    <script>
      function s3UploadFile(preSignedURL, file) {
        return axios.put(preSignedURL, file, { headers: {
          'Content-Type': 'multipart/form-data',
        } })
      }
      const app = new Vue({
        el: '#app',
        data: {
          file: '',
          title: '',
          description: '',
          imagePosts: [],
        },
        computed: {},
        methods: {
          upload() {
            let url = null
            axios.post(`/generatePresignedUrl`, {
              filename: document.getElementById('f').files[0].name
            })
              .then(({ data }) => {
                url = data.url
                return s3UploadFile(data.presignedUrl, document.getElementById('f').files[0])
              })
              .then(({ data }) => {
                return axios.post(`/images`, {
                  title: this.title,
                  description: this.description,
                  url,
                })
              }).then(({ data }) => {
                this.fetchImagePosts()
              })
              .catch((err) => {
                console.log(document.getElementById('f').files[0])
                console.log(err)
              })
          },
          fetchImagePosts() {
            axios.get(`/images`)
              .then(({ data }) => {
                this.imagePosts = data
              })
          }
        },
        mounted() {
          this.fetchImagePosts()
        }
      })
    </script>
  </body>
</html>
```  
  
## Express 서버에서 정적 호스팅하기  
아래의 코드를 `index.js`에 추가합니다.  
  
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
        url: `https://s3-ap-southeast-1.amazonaws.com/${params.Bucket}/${key}`,
        presignedUrl,
    })
})
///////// 여기서부터
app.use(express.static('public'))  
///////// 여기까지 추가

app.listen(3000)
```  
  
- Cloud9 엔드포인트 IP 주소 뒤에 index.html을 붙여서 브라우저에 넣어보세요!  
> 예: 13.250.63.64:3000/index.html  
- 동작하는 앱을 확인 하실 수 있습니다.  
  
수고많으셨습니다. 오늘 실습은 여기까지입니다! 감사합니다.
<br>
다음 [삭제 가이드](../6_Delete) 를 따라서 삭제를 진행해주세요.
