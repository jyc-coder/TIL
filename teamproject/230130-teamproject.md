
# teamp project api 
팀프로젝트에 사용될 api에 대한 설명을 진행
그중에 회원가입과 관련된 기능을 시연해주셨음.

```js
const idEl = document.querySelector('.id')
const passwordEl = document.querySelector('.password')
const displayNameEl = document.querySelector('.display-name')
const inputFileEl = document.querySelector('input[type="file"]')
const submitEl = document.querySelector('.signup')
const authorizationEl = document.querySelector('.authorization')

let id = ''
let pw = ''
let displayName = ''
let profileImgBase64 = ''

idEl.addEventListener('input', (event) => {
  id = event.target.value
})
passwordEl.addEventListener('input', (event) => {
  pw = event.target.value
})
displayNameEl.addEventListener('input', (event) => {
  displayName = event.target.value
})
inputFileEl.addEventListener('change', (event) => {
  const file = inputFileEl.files[0]
  console.log(file)
  const reader = new FileReader()
  reader.readAsDataURL(file)
  reader.addEventListener('load', (e) => {
    console.log(e.target.result) // base64
    profileImgBase64 = e.target.result
  })
})
submitEl.addEventListener('click', () => {
  console.log(id, pw, displayName, profileImgBase64)
  request({
    url: 'https://asia-northeast3-heropy-api.cloudfunctions.net/api/auth/signup',
    method: 'POST',
    data: {
      email: id,
      password: pw,
      displayName: displayName,
      profileImgBase64: profileImgBase64,
    },
  })
})
authorizationEl.addEventListener('click', () => {
  request({
    url: 'https://asia-northeast3-heropy-api.cloudfunctions.net/api/auth/me',
    method: 'POST',
    headers: {
      Authorization: 'Bearer <access_token>',
      masterKey: true,
    },
  })
})

async function request(options) {
  const defaultOptions = {
    method: 'GET',
    headers: {
      'content-type': 'application/json',
      apikey: 'FcKdtJs202301',
      username: 'KDT0_TEAM0',
    },
  }
  const headers = options.headers || {}
  const res = await fetch(options.url, {
    method: options.method || defaultOptions.method,
    headers: {
      ...defaultOptions.headers,
      ...headers,
    },
    body: options.data ? JSON.stringify(options.data) : undefined,
  })
  const json = await res.json()
  console.log(json)
}

```


# 팀프로젝트에 대한 충고

## 여러 의견이 나왔을때 결정은 결국 조장이 한다.
물론 당연한 소리이지만 문제는 바로 그 다음이다. 만약 선택되지 못한 의견이 타당한 근거를 가지고 선택되지 못했거나, 혹은 그에 대한 설득이 제대로 이루어지지 않게 된다면, 이에대한 불만은 지속적으로 쌓여가게 되며 이는 곧 팀워크에 문제가 생기가 된다.

오해와 문제는 빨리 풀어라.


## 팀프로젝트 만드는 여정을 기록할 것 

프로젝트를 진행한 것은 누구나 다 했다. 이 길로 취업하려는 사람들은 죄다 이런것들 만들었을 테니 변별력은 없다.
중요한 것은 바로 만들면서 어떤 시행 착오를 겪었고, 이를 어떤 방식으로 해결했는지의 과정이다.
이를 포트폴리오나 이력서에 기록해주면, 이 사람이 문제가 발생했을때 어떤 방식으로 해결하는 사람이라는 것을 파악할수 있게 해줄 것이다. 
그냥 프로젝트만 올리면 어떤 여정을 거쳤는지 알수 없다.


험난한 여정을 스토리 텔링해라. 아주 볼만할 것이다.

