# API说明

智能舌象识别API主要用于识别舌象中的多维特征点，返回舌象特征识别结果、各个特征点对应的中医含义以及部分舌象特征的可视化分割图。其中，舌象的特征点包括舌色、苔色、点刺、瘀点、瘀斑、裂纹、齿痕、舌尖、舌形、苔质、津液、舌下脉络共12维的特征。具体的检测表如下所示：

| **序号** | **舌象名称** | **特征类别** |
| :--- | :--- | :--- |
| 1 | 舌色 | 淡白舌,淡紫舌,青紫舌,淡红舌,暗红舌,红舌, |
| 2 | 苔色 | 淡黄苔,白苔,黄苔,灰黑苔无苔 |
| 3 | 点刺 | 有点刺,未见点刺 |
| 4 | 瘀点 | 有瘀点,未见瘀点 |
| 5 | 瘀斑 | 有瘀斑,未见瘀斑 |
| 6 | 裂纹 | 有裂纹,未见裂纹 |
| 7 | 齿痕 | 有齿痕,未见齿痕 |
| 8 | 舌尖 | 红,正常 |
| 9 | 舌形 | 胖大舌,瘦薄舌,娇嫩舌,苍老舌,歪斜舌,尖舌,正常 |
| 10 | 苔质 | 厚苔,无苔,腐苔,腻苔,有剥落,薄苔 |
| 11 | 津液 | 润,燥,滑 |
| 12 | 舌下脉络 | 正常,血瘀,未见舌下 |

# 调用地址

> [http://chunfengai.top:8000/cv/baogao](http://chunfengai.top:8000/cv/baogao)

# **请求体**

以json的形式发送请求，api接口接受GET或POST请求：

```py
{
    'image_0':"base64///////",  #必要的 舌面照片
    'image_1':"base64///////",  #非必要的 舌底照片
    'age':20,
    'weight':65,  
    'height':180,
    'sex':'M'/'F'
}
```

| 名称 | 类别 | 备注 |
| :--- | :--- | :--- |
| image\_0 | string | 必须有 舌面照片的base64编码 |
| image\_1 | string | 舌下照片的base64编码 |
| weight | int | 单位是kg |
| height | int | 单位是cm |
| sex | string | M/F |

# **返回示例**

```py
{
    "checkList": {
        "tongueColor": "青紫舌",
        "mossColor": "灰黑苔",
        "prick": "有点刺",
        "indentation": "有齿痕",
        "flaw": "有裂纹",
        "petechia": "有瘀点",
        "ecchymosis": "有瘀斑",
        "tipTongue": "红",
        "bodyFluid": "滑",
        "sublingualCollaterals": "正常",
        "tongueShaped": "胖大舌,苍老舌,尖舌",
        "coatingNature": "腻苔,薄苔,有剥落"
    },
    "report": {
        "tongueShapeList": [
            {
                "key": "点刺舌",
                "desc": "点刺舌：通常表示有热毒在体或血热。",
                "pic": [
                    {
                        "name": "健康舌象图",
                        "file": "base64正常"
                    },
                    {
                        "name": "AI检测点刺图",
                        "file": "base64检测"
                    }
                ]
            },
            {
                "key": "齿痕舌",
                "desc": "齿痕舌：是指舌边有齿印，常与脾虚导致的水肿有关。",
                "pic": [
                    {
                        "name": "健康舌象图",
                        "file": "base64正常"
                    },
                    {
                        "name": "AI检测齿痕图",
                        "file": "base64检测"
                    }
                ]
            },
            {
                "key": "裂纹舌",
                "desc": "裂纹舌：多见于阴虚火旺或气血两亏。",
                "pic": [
                    {
                        "name": "健康舌象图",
                        "file": "base64正常"
                    },
                    {
                        "name": "AI检测裂纹图",
                        "file": "base64检测"
                    }
                ]
            },
            {
                "key": "瘀点舌",
                "desc": "瘀点舌：主要是血液循环不畅，可能有瘀血。",
                "pic": [
                    {
                        "name": "健康舌象图",
                        "file": "base64正常"
                    },
                    {
                        "name": "AI检测瘀点图",
                        "file": "base64检测"
                    }
                ]
            },
            {
                "key": "瘀斑舌",
                "desc": "瘀斑舌：主要是血液循环不畅，可能有瘀血。",
                "pic": [
                    {
                        "name": "健康舌象图",
                        "file": "base64正常"
                    },
                    {
                        "name": "AI检测瘀斑图",
                        "file": "base64检测"
                    }
                ]
            },
            {
                "key": "胖大舌",
                "desc": "胖大舌：可能是因为脾胃湿痰过盛或脾虚导致水液代谢不良。"
            },
            {
                "key": "苍老舌",
                "desc": "苍老舌：可能因为气血虚弱，肾精亏损。"
            },
            {
                "key": "尖舌",
                "desc": "尖舌：多指心脾两虚，或是心火过旺。"
            }
        ],
        "coatingNatureList": [
            {
                "key": "腻苔",
                "desc": "腻苔：舌苔厚腻，表明身体内有湿热或痰湿。"
            },
            {
                "key": "薄苔",
                "desc": "薄苔：通常为正常表现，若伴有病状可能表示病情较轻或疾病初起。"
            },
            {
                "key": "有剥落",
                "desc": "有剥落：通常与肾阴虚或胃热有关。"
            }
        ],
        "tongueColor": {
            "key": "青紫舌",
            "desc": "青紫舌：通常表示寒热错杂、气滞血瘀。"
        },
        "mossColor": {
            "key": "灰黑苔",
            "desc": "灰黑苔：可能是体内有热毒、血瘀或是严重的寒湿凝滞。"
        },
        "tipTongue": {
            "key": "红",
            "desc": "舌尖红：多半是心火旺盛或心阴不足。"
        },
        "bodyFluid": {
            "key": "津液滑",
            "desc": "津液滑：常见于食物滞留或湿热内蕴。"
        },
        "sublingualCollaterals": {
            "key": "舌下正常",
            "desc": "舌下正常：舌象脉络正常表示血液循环良好，没有明显的气滞血瘀或者内热现象。"
        }
    },
    "report_desc": "以中医视角分析该舌面、舌底照片，我们可以观察到：\n1、舌色：青紫色的舌体。青紫色舌质通常表示寒热错杂、气滞血瘀。根据具体情况活血化瘀或温里散寒，如血府逐瘀汤或苏合香丸。\n2、舌形：舌体胖大，舌质苍老，舌形为尖舌，有点刺，且舌尖较红，舌面上有瘀点或瘀斑，舌两侧齿痕明显。胖大舌可能是因为脾胃湿痰过盛或脾虚导致水液代谢不良。苍老舌可能因为气血虚弱，肾精亏损。尖舌多指心脾两虚，或是心火过旺。裂纹多见于阴虚火旺或气血两亏。齿痕是指舌边有齿印，常与脾虚导致的水肿有关。瘀点或瘀斑主要是血液循环不畅，可能有瘀血。点刺通常表示有热毒在体或血热。舌尖红多半是心火旺盛或心阴不足。\n3、舌苔：灰黑色的舌苔，舌苔正常，并且有剥落，有些腐腻。灰黑色舌苔可能是体内有热毒、血瘀或是严重的寒湿凝滞。应忌食油腻、生冷和难以消化的食物。腻苔舌苔厚腻，表明身体内有湿热或痰湿。薄苔通常为正常表现，若伴有病状可能表示病情较轻或疾病初起。有剥落通常与肾阴虚或胃热有关。\n4、津液：津液较滑。津液滑常见于食物滞留或湿热内蕴。减少油腻、生冷、难消化的食物的摄入。\n5、舌下脉络：舌下脉络正常。舌象脉络正常表示血液循环良好，没有明显的气滞血瘀或者内热现象。平日注意情绪稳定，避免过度劳累。\n\n上述分析不作为医疗诊断参考依据，舌诊应该由有经验的中医医生进行。以下是详细的舌诊分析报告。"
}
```

返回的JSON中一共包括三个key：

**checkList**：舌象12维特征的检测识别结果，以JSON形式返回。

**report**：包含舌象特征检测识别结果及对应特征的中医含义，同时也包括裂纹、齿痕、瘀点、瘀斑的特征分割图，以JSON形式返回。

**report\_desc**：舌象识别之后的报告描述文本，文本中包括该舌象图片的所有特征描述，以String形式返回。

## **checkList中的key和value**

| **序号** | **报告项目** | **对应key** | **类型** | value数据枚举 |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 舌色 | tongueColor | String | "淡白舌","淡紫舌","青紫舌","淡红舌","暗红舌","红舌", |
| 2 | 苔色 | mossColor | String | "淡黄苔","白苔","黄苔","灰黑苔""无苔" |
| 3 | 点刺 | prick | String | "有点刺""未见点刺" |
| 4 | 瘀点 | petechia | String | "有瘀点""未见瘀点" |
| 5 | 瘀斑 | ecchymosis | String | "有瘀斑""未见瘀斑" |
| 6 | 裂纹 | flaw | String | "有裂纹""未见裂纹" |
| 7 | 齿痕 | indentation | String | "有齿痕""未见齿痕" |
| 8 | 舌尖 | tipTongue | String | "红""正常" |
| 9 | 舌形 | tongueShaped | String | 胖大舌,瘦薄舌,娇嫩舌,苍老舌,歪斜舌,尖舌,正常 |
| 10 | 苔质 | coatingNature | String | 厚苔,无苔,腐苔,腻苔,有剥落,薄苔 |
| 11 | 津液 | bodyFluid | String | 润,燥,滑 |
| 12 | 舌下脉络 | sublingualCollaterals | String | "正常" 血瘀" "未见舌下" |

## report中的key和value 

| **序号** | **名称** | **对应key** | **类型** | **备注** |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 舌形 | tongueShapeList | List | 以JSON形式返回该图片中出现的每一种舌形以及其对应的描述，以及AI检测的结果图片（如有）。 |
| 2 | 舌苔 | coatingNatureList | List | 以JSON的形式返回该图片中出现的每一种舌苔特征以及其对应的描述。 |
| 3 | 苔色 | mossColor | JSON | 苔色字典，包括苔色和其对应的描述。 |
| 4 | 舌色 | tongueColor | JSON | 舌色字典，包括舌色和其对应的描述。 |
| 5 | 舌尖 | tipTongue | JSON | 舌尖特征字典，包括舌尖特征和其对应的描述。 |
| 6 | 津液 | bodyFluid | JSON | 津液特征字典，包括津液特征和其对应的描述。 |
| 7 | 舌下脉络 | sublingualCollaterals | JSON | （可选）（如上传舌下照片则有）舌下脉络特征字典，包括舌下脉络特征和其对应的描述。 |



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
url = 'http://chunfengai.top:8000/cv/baogao'
img_path_shemian = 'sheshang.jpeg'
img_path_shexia = 'shexia.jpeg'
headers = {
    'Authorization': authorization_token #这里是加密key
}
with open(img_path_shemian, 'rb') as f:
    base64image_shemian = base64.b64encode(f.read()).decode('utf-8')
with open(img_path_shexia, 'rb') as f:
    base64image_shexia = base64.b64encode(f.read()).decode('utf-8')
request_param = {'image_0':base64image_shemian,'image_1':base64image_shexia} 
req = requests.post(url, json=json.dumps(request_param),headers=headers)
print(req.json())
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
// 引入axios库，如果尚未安装，请使用npm install axios进行安装
const axios = require('axios');

// 定义URL和headers
const url = 'http://chunfengai.top:8000/cv/baogao';
const headers = {
    'Authorization': 'authorization_token' // 这里替换为您的加密key
};

// 定义base64编码的图片数据，这里需要替换为您的实际图片数据
const base64image_shemian = 'base64image_shemian'; // 请替换为您的实际base64图片数据
const base64image_shexia = 'base64,base64image_shexia'; // 请替换为您的实际base64图片数据

// 构建请求参数
const requestParam = {
    'image_0': base64image_shemian,
    'image_1': base64image_shexia
};

// 发送POST请求
axios.post(url, requestParam, { headers: headers })
    .then(response => {
        // 请求成功，打印响应数据
        console.log(response.data);
    })
    .catch(error => {
        // 请求失败，打印错误信息
        console.error('Error during request to ' + url, error);
    });
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

# 定义URL和Authorization token
url='http://chunfengai.top:8000/cv/baogao'
authorization_token='your_authorization_token_here' # 替换为您的加密key

# 图片文件路径
img_path_shemian='sheshang.jpeg'
img_path_shexia='shexia.jpeg'

# 读取图片文件并转换为base64编码
base64image_shemian=$(base64 $img_path_shemian | tr -d '\n')
base64image_shexia=$(base64 $img_path_shexia | tr -d '\n')

# 构建请求参数
request_param=$(jq -n \
  --arg img0 "$base64image_shemian" \
  --arg img1 "$base64image_shexia" \
  '{image_0: $img0, image_1: $img1}')

# 发送POST请求
response=$(curl -s -o /dev/null -w "%{http_code}" -X POST \
  -H "Authorization: $authorization_token" \
  -H "Content-Type: application/json" \
  -d "$request_param" \
  "$url")

# 打印响应状态码
echo "Response Status Code: $response"

# 如果需要打印响应内容，可以取消下面注释行
# echo "Response Content: $response"
```



