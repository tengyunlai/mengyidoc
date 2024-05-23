# API说明

萌医大模型以GPT舌诊技术为切入口，通过对舌色、苔色、点刺、瘀点、瘀斑、裂纹、齿痕等多达20个维度的细致分析，结合患者的基本信息、身体症状及疾病史，实现对患者健康状况、体质的精准评估。其次，系统还包括了对检测检验报告的智能解读，帮助医生、患者更快更精准的理解和应用报告结果；此外，针对春播特有的中医贴敷技术、体质调理方案进行专业化的定制，因此中医大模型可根据患者的主诉、疾病史、舌脉、检测检验报告等多个维度进行综合分析，提供贴敷治疗、体质调理、经典方剂等多方面的辅助治疗建议。

# 流式调用地址

> http://chunfengai.top:8000/bigmodel/stream


# **流式调用请求体**

以json的形式发送请求，api接口接受POST请求：


| 名称               | 说明                                                         | 类型          | 是否可空         | 示例                                                         |
| :----------------- | :----------------------------------------------------------- | :------------ | :--------------- | :----------------------------------------------------------- |
| messages           | 存放历史对话                                                 | array of dict | 否               | [{"sender_type": "USER","text": "【舌象特征描述】：从舌色、舌形、舌苔、津液等多个角度分析该舌象，各个特点如下：\n1、舌色：舌质的颜色是淡紫色。\n2、舌形：舌形为胖舌，舌尖正常。\n3、舌苔：舌苔的颜色是白色，舌苔薄。\n4、津液：该舌象的津液特征是滑。我可能是什么体质？"},{"sender_type": "BOT","text": ""}] |
| max_tokens         | 控制单轮最大输出长度                                         | int           | 是               | 2048                                                         |
| max_history_tokens | 控制历史对话的最大长度，超过了会自动截断之前的会话           | int           | 是               | 4096                                                         |
| temperature        | 控制回答生成的多样性，越大生成的不确定性越强，多样性越高，越小多样性越低 | float         | 是               | 0.2                                                          |
| top_p              | 控制beam search的多样性，越大生成的不确定性越强，多样性越高，越小多样性越低 | float         | 是               | 0.9                                                          |
| do_sample          | 是否进行答案的采样，用于控制随机性                           | bool          | 是               | true                                                         |
| return_prob        | 是否返回每个token的概率                                      | bool          | 是               | false                                                        |
| requestId          | 请求id，全局唯一，用于区分不同请求                           | string        | 是，但不建议为空 | 123                                                          |
| system_prompt      | 不同场景的prompt                                             | string        | 是               | I'm an AI asssistant to provide accurate, polite, detailed, comprehensive, positive and friendly answers to users' questions.\n我是一款人工智能助手,我的目标是根据用户所提出的问题,给予精确、礼貌、细致、丰富、积极、友善的回复。\n |

```bash
#请求体：
{"max_tokens":2048,"messages":[{"sender_type":"USER","text":"【舌象特征描述】：从舌色、舌形、舌苔、津液等多个角度分析该舌象，各个特点如下：\n1、舌色：舌质的颜色是淡紫色。\n2、舌形：舌形为胖舌，舌尖正常。\n3、舌苔：舌苔的颜色是白色，舌苔薄。\n4、津液：该舌象的津液特征是滑。我可能是什么体质？"},{"sender_type":"BOT","text":""}],"do_sample":false,"top_p":0.9,"temperature":0.2,"top_k":null,"return_prob":false}
```

# 流式调用**返回**

| 名称       | 说明                               | 类型            | 示例                                                         |
| :--------- | :--------------------------------- | :-------------- | :----------------------------------------------------------- |
| isSuccess  | 总体调用状态                       | bool            | TRUE                                                         |
| requestId  | 请求id，全局唯一，用于区分不同请求 | string          | 123                                                          |
| version    | 接口版本                           | string          | v0.0.1                                                       |
| reply      | 当前轮次的回复，仅非流式有效       | string          | 根据您的舌象特征,可能存在以下几种体质:阳虚质、气虚质和血瘀质 |
| choices    | 模型流式输出，仅流式有效           | string          | {"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "呢?", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"} |
| llm_status | 大模型调用状态                     | string          | success                                                      |
| reason     | 停止原因                           | string          | success                                                      |
| guide      | 推荐的问题                         | array of string | ["是的,我经常感到神疲乏力、气短、畏寒肢冷,容易出汗。", "是的,我经常感到容易疲劳、气短、面色黄或淡白、目光少神。", "为什么我的舌色是淡紫色?"] |
| guide      | 总体调用状态                       | bool            | TRUE                                                         |
| probs      | 每个token的概率                    | array of dict   | []                                                           |
| message    | 错误提示                           | string          | request data is not json format                              |


```bash：
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "根据您的舌象特征,可能存在以下几种体质:阳虚质、气虚质和血瘀质。淡紫色的舌质可能暗示着血液循环不畅,这可能是血瘀质的表现。舌形为胖舌,舌苔的颜色是白色,舌苔薄,津液特征是滑,这些特征更接近阳虚质和气虚质。然而,体质不仅与舌象有关,还与身体症状密切相关。为了更准确地判断您的体质,我需要了解更多信息。您是否感到神疲乏力、气短、畏寒肢冷,容易出汗?或者您是否感到容易疲劳、气短、面色黄或淡白、目光少神?", "choices": null, "llm_status": "success", "reason": "success", "guide": ["是的,我经常感到神疲乏力、气短、畏寒肢冷,容易出汗。", "是的,我经常感到容易疲劳、气短、面色黄或淡白、目光少神。", "为什么我的舌色是淡紫色?"], "probs": null, "message": "ok"}

```



# **调用代码（流式）--python**

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

单轮对话

```python
import requests
import json
import sys
import time

url = 'http://chunfengai.top:8000/bigmodel/stream'

class Demo():
    def __init__(self, url, authorization_token):
        self.url = url
        self.authorization_token = authorization_token

    def process_input(self, text, history=[], max_tokens=2048, temperature=0.2, top_k=0, top_p=0.9):
        messages = []
        for chat in history:
            messages.append({"sender_type": "USER", "text": chat[0]})
            messages.append({"sender_type": "BOT", "text": chat[1]})
        added = [
            {"sender_type": "USER", "text": text},
            {"sender_type": "BOT", "text": ""},
        ]
        messages.extend(added)
        req_body = {
            "messages": messages,
            "max_tokens": max_tokens,
            "temperature": temperature,
            "top_k": top_k,
            "top_p": top_p
        }
        return req_body

    def stream(self, text, history=[], max_tokens=2048, temperature=0.2, top_k=0, top_p=0.9):
        url = f'{self.url}'
        req_body = self.process_input(
            text=text,
            history=history,
            max_tokens=max_tokens,
            temperature=temperature,
            top_k=top_k,
            top_p=top_p
        )
        headers = {
            'Authorization': self.authorization_token  # 这里是加密key
        }
        r = requests.post(url, json=req_body, headers=headers, stream=True)

        resp_text = ""
        for chunk in r.iter_lines():
            if len(chunk.strip()) < 2:
                continue
            try:
                chunk = json.loads(chunk.strip())
            except Exception as e:
                print(e)
                continue

            if chunk['llm_status'] != "success":
                yield "很抱歉，我并没有与这个问题相关的信息，如果您还有其它问题，请向我提问，我会努力回答的～\n请重新调用 API"
                return
            if chunk['choices']:
                resp_text += chunk['choices'][0]['delta']
                yield chunk['choices'][0]['delta']
                if chunk['choices'][0]['finish_reason'] == "stop":
                    break


if __name__ == '__main__':
    d = Demo(url, "{authorization_token}")
    text = "背诵雨霖铃"
    history = []
    for resp in d.stream(text, history):
        print(resp, end="")
        sys.stdout.flush()
        time.sleep(0.1)
```

多轮对话

```python
import requests
import json
import sys
import time

url = 'http://chunfengai.top:8000/bigmodel/stream'

import requests
import json
import sys
import time


class Demo():
    def __init__(self, url, authorization_token):
        self.url = url
        self.authorization_token = authorization_token
        self.history = []

    def process_input(self, text, max_tokens=2048, temperature=0.2, top_k=0, top_p=0.9):
        messages = []
        for chat in self.history:
            messages.append({"sender_type": "USER", "text": chat[0]})
            messages.append({"sender_type": "BOT", "text": chat[1]})
        added = [
            {"sender_type": "USER", "text": text},
            {"sender_type": "BOT", "text": ""},
        ]
        messages.extend(added)
        req_body = {
            "messages": messages,
            "max_tokens": max_tokens,
            "temperature": temperature,
            "top_k": top_k,
            "top_p": top_p
        }
        return req_body

    def stream(self, text, max_tokens=2048, temperature=0.2, top_k=0, top_p=0.9):
        url = self.url
        req_body = self.process_input(
            text=text,
            max_tokens=max_tokens,
            temperature=temperature,
            top_k=top_k,
            top_p=top_p
        )
        headers = {
            'Authorization': self.authorization_token  # 这里是加密key
        }
        r = requests.post(url, json=req_body, headers=headers, stream=True)

        resp_text = ""
        for chunk in r.iter_lines():
            if len(chunk.strip()) < 2:
                continue
            try:
                chunk = json.loads(chunk.strip())
            except Exception as e:
                print(e)
                continue

            if chunk['status'] != "success":
                yield "很抱歉，我并没有与这个问题相关的信息，如果您还有其它问题，请向我提问，我会努力回答的～\n请重新调用 API"
                return

            resp_text += chunk['choices'][0]['delta']
            yield chunk['choices'][0]['delta']
            if chunk['choices'][0]['finish_reason'] == "stop":
                self.history.append([text, resp_text])
                break


if __name__ == '__main__':
    d = Demo(url, "{authorization_token}")
    for i in range(3):
        print("[USER]: ")
        text = f"背诵出师表第{i+1}段"
        print(text)
        if text is None or text.strip() == "":
            break
        print("[BOT]: ")
        for resp in d.stream(text.strip()):
            print(resp, end="")
            sys.stdout.flush()
            time.sleep(0.1)
        print("")
```


# **调用代码（流式）--Bash**

第一步鉴权，获得加密key：

```bash
curl --location --request GET 'http://chunfengai.top:8000/auth/login' \
    --form 'username="***你的username***"' \
    --form 'password="***你的password***"'
```


第二步调用，获得识别结果：

```bash
#!/bin/bash
curl  http://chunfengai.top:8000/bigmodel/stream --header 'Authorization: ***你的加密key***'  -XPOST -d "{\"max_tokens\":2048,\"messages\":[{\"sender_type\":\"USER\",\"text\":\"【舌象特征描述】：从舌色、舌形、舌苔、津液等多个角度分析该舌象，各个特点如下：\n1、舌色：舌质的颜色是淡紫色。\n2、舌形：舌形为胖舌，舌尖正常。\n3、舌苔：舌苔的颜色是白色，舌苔薄。\n4、津液：该舌象的津液特征是滑。我可能是什么体质？\"},{\"sender_type\":\"BOT\",\"text\":\"\"}],\"do_sample\":false,\"top_p\":0.9,\"temperature\":0.2,\"top_k\":null,\"return_prob\":false}"
```

接口返回：

```bash
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "根据您的舌象特征,淡", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "紫色的舌质可能对应阳", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "虚质、气虚质或血瘀质", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": ",胖舌可能对应阳虚质", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "或气虚质,白色薄苔可能", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "对应阳虚质或气虚质,", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "津液滑可能对应阳虚质", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "。但是,体质的判断不仅", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "与舌象有关,还与身体", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "症状相关。为了进一步", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "确认您的体质,我需要", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "了解更多信息。请问您", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "是否经常感到疲劳、气", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "短,或者记忆力差、皮肤", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "偏暗?", "finish_reason": ""}], "llm_status": "success", "reason": "success", "guide": [], "probs": null, "message": "ok", "status": "success"}
{"isSuccess": true, "requestId": "", "version": "v0.0.1", "reply": "", "choices": [{"delta": "", "finish_reason": "stop"}], "llm_status": "success", "reason": "success", "guide": ["我经常感到疲劳、气短。", "我记忆力差、皮肤偏暗。", "为什么我的舌苔是白色的?"], "probs": null, "message": "ok", "status": "success"}
 
```

