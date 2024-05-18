# API说明

萌医语音识别大模型旨在通过ASR技术识别用户的声音，将语音音频文件转换成文字

# 调用地址

> http://chunfengai.top:8000/asr/rec

# **请求体**

以json的形式发送请求，api接口接受GET或POST请求：

```py
{
    'audio':"audio base64 encode///////",  #必要的
}
```

| 名称 | 类别 | 备注 |
| :--- | :--- | :--- |
| Audio | string | 必须有 音频文件的base64编码 |

# **返回**

```py
{
    'code': 200,    
    'ori_res':'萌医语音识别大模型',
    'results': '萌医语音识别大模型'
}
```

| 名称 | 类型 | 备注 |
| :--- | :--- | :--- |
| code | int | 成功200 错误500 |
| ori_res | string | 语音模型结果 |
| results | string | 萌医语音大模型语音识别结果 |

## 注意：

1. 只接受小于或等于1分钟的音频文件，若上传的音频文件大于1分钟，则返回错误。
2. 只接受mp3，wav音频编码格式的音频文件，无法读取acc

 

# **调用代码--python**

第一步鉴权，获得加密key：

```python
import base64,json,requests
# 以下为鉴权部分
username = "***username***" # 替换成你的用户名
password = "***password***" # 替换成你的密码

# 定义登录数据
login_data = {
'username': username,
'password': password
}
# 发送 POST 请求
response = requests.post('http://chunfengai.top:8000/auth/login', data=login_data)
authorization_token = response.json()['data']['token'] #这里是加密key
```

第二步调用，获得识别结果：

```python
#以下为获得key之后的调用部分
url = 'http://chunfengai.top:8000/asr/rec'
audio_path = 'output.wav'
headers = {
    'Authorization': authorization_token #这里是加密key
}
with open(audio_path, 'rb') as f:
    base64audio = base64.b64encode(f.read()).decode('utf-8') 
request_param = {'audio':base64audio} 
req = requests.post(url, json=json.dumps(request_param),headers=headers)
print(req.json())
# 输出：{'code': 200,    'ori_res':'萌医语音识别大模型','results': '萌医语音识别大模型'}
```

# **调用代码--JS**

第一步鉴权，获得加密key：

```js
// 以下为鉴权部分
const username = "***username***"; // 替换成你的用户名
const password = "***password***"; // 替换成你的密码

// 定义登录数据
const login_data = {
  'username': username,
  'password': password
};

// 发送 POST 请求
fetch('http://chunfengai.top:8000/auth/login', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(login_data)
})
.then(response => {
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
})
.then(data => {
  // 假设响应的数据结构为 { "data": { "token": "your_token" } }
  const authorization_token = data.data.token;
  console.log('Authorization token:', authorization_token);

  // 接下来可以使用 authorization_token 进行其他需要认证的请求
})
.catch(error => {
  // 打印错误信息
  console.error('There has been a problem with your fetch operation:', error);
});
```

第二步调用，获得识别结果：

```js
const fetch = require('node-fetch'); // 使用node-fetch库来发送HTTP请求
const fs = require('fs'); // 使用fs库来读写文件
const { promisify } = require('util'); // 用于将回调函数转换为Promise
const readFileAsync = promisify(fs.readFile); // 将fs.readFile转换为Promise形式

const url = 'http://chunfengai.top:8000/asr/rec';
const audioPath = 'output.wav';
const headers = {
    'Authorization': authorizationToken // 这里的变量名改为camelCase以符合JavaScript的命名习惯
};

(async () => {
    try {
        // 读取文件并转换为Base64编码
        const fileBuffer = await readFileAsync(audioPath);
        const base64Audio = fileBuffer.toString('base64');
        
        // 设置请求参数
        const requestParam = { audio: base64Audio };
        
        // 发送POST请求
        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Authorization': headers['Authorization'],
                'Content-Type': 'application/json' // 设置请求头以告知发送的是JSON数据
            },
            body: JSON.stringify(requestParam) // 请求体为JSON字符串
        });
        
        // 解析响应内容
        const result = await response.json();
        console.log(result);
    } catch (error) {
        console.error('请求失败:', error);
    }
})();
```

# **调用代码--Bash**

第一步鉴权，获得加密key：

```bash
curl --location --request GET 'http://chunfengai.top:8000/auth/login' \
    --form 'username="***你的username***"' \
    --form 'password="***你的password***"'
```

第二步调用，获得识别结果：

```bash
#!/bin/bash

# 替换成您的模型接口地址和加密key
url='http://chunfengai.top:8000/asr/rec'
audio_path='1.wav'
authorization_token='你的授权token' # 替换成您的授权token

# 读取图像文件并进行 base64 编码
base64audio=$( base64 "$audio_path" )

# 构建请求参数
request_param=$( jq -n \
  --arg audio "$base64audio" \
  '{
    audio: $audio,
  }' )

# 发送 POST 请求
curl -X POST "$url" \
     -H "Authorization: $authorization_token" \
     -H "Content-Type: application/json" \
     -d "$request_param"

# 如果需要检查响应，可以添加响应处理的代码
```

