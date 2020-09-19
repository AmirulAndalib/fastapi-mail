
# Fastapi-mail

The fastapi-mail simple lightweight mail system, sending emails and attachments(individual && bulk)


[![MIT licensed](https://img.shields.io/github/license/marlin-dev/fastapi-mail)](https://raw.githubusercontent.com/marlin-dev/fastapi-mail/master/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/marlin-dev/fastapi-mail.svg)](https://github.com/marlin-dev/fastapi-mail/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/marlin-dev/fastapi-mail.svg)](https://github.com/marlin-dev/fastapi-mail/network)
[![GitHub issues](https://img.shields.io/github/issues-raw/marlin-dev/fastapi-mail)](https://github.com/marlin-dev/fastapi-mail/issues)
[![Downloads](https://pepy.tech/badge/fastapi-mail)](https://pepy.tech/project/fastapi-mail)


###  🔨  Installation ###

```sh
 $ pip install fastapi-mail
```


### In order to run the application use command below ####

```sh
uvicorn test_examples.main:app --reload  --port 8001

```

### Guide


```python

from fastapi import FastAPI, BackgroundTasks, UploadFile, File, Form
from starlette.responses import JSONResponse
from starlette.requests import Request
from fastapi_mail import FastMail, MessageSchema,ConnectionConfig
from pydantic import EmailStr
from pydantic import EmailStr, BaseModel
from typing import List



class EmailSchema(BaseModel):
    email: List[EmailStr]


conf = ConnectionConfig(
    MAIL_USERNAME = "your@email.com",
    MAIL_PASSWORD = "strong_password",
    MAIL_PORT = 587,
    MAIL_SERVER = "your mail server",
    MAIL_TLS = True,
    MAIL_SSL = False
)

app = FastAPI()


html = """
<html> 
<body>
<p>Hi This test mail
<br>Thanks for using Fastapi-mail</p> 
</body> 
</html>
"""

template = """
<html> 
<body>
<p>Hi This test mail using BackgroundTasks
<br>Thanks for using Fastapi-mail</p> 
</body> 
</html>
"""


@app.post("/email")
async def simple_send(email: EmailSchema) -> JSONResponse:

    message = MessageSchema(
        subject="Fastapi-Mail module",
        receipients=email.dict().get("email"),  # List of receipients, as many as you can pass 
        body=html,
        subtype="html"
        )

    fm = FastMail(conf)
    await fm.send_message(message)
    return JSONResponse(status_code=200, content={"message": "email has been sent"})
        

```

### Sending email as background task

```python
   
@app.post("/emailbackground")
async def send_in_background(background_tasks: BackgroundTasks,email: EmailSchema) -> JSONResponse:

    message = MessageSchema(
        subject="Fastapi mail module",
        receipients=email.dict().get("email"),
        body="Simple background task ",
        )

    fm = FastMail(conf)
    
    background_tasks.add_task(fm.send_message,message)

    return JSONResponse(status_code=200, content={"message": "email has been sent"})


```


### Sending files


```python

@app.post("/file")
async def send_file(background_tasks: BackgroundTasks,file: UploadFile = File(...),email:EmailStr = Form(...)) -> JSONResponse:

    message = MessageSchema(
            subject="Fastapi mail module",
            receipients=[email],
            body="Simple background task ",
            attachments=[file]
            )

    fm = FastMail(conf)
        
    background_tasks.add_task(fm.send_message,message)

    return JSONResponse(status_code=200, content={"message": "email has been sent"})



```


# Contributing
Fell free to open issue and send pull request.


## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/marlin-dev"><img src="https://avatars.githubusercontent.com/u/46589585?v=3" width="100px;" alt=""/><br /><sub><b>Sabuhi Shukurov</b></sub></a><br /><a href="#question-kentcdodds" title="Answering Questions">💬</a> <a href="https://github.com/all-contributors/all-contributors/commits?author=kentcdodds" title="Documentation">📖</a> <a href="https://github.com/all-contributors/all-contributors/pulls?q=is%3Apr+reviewed-by%3Akentcdodds" title="Reviewed Pull Requests">👀</a> <a href="#talk-kentcdodds" title="Talks">📢</a></td>
    <td align="center"><a href="https://github.com/Turall"><img src="https://avatars.githubusercontent.com/u/32899328?v=3" width="100px;" alt=""/><br /><sub><b>Tural Muradov</b></sub></a><br /><a href="https://github.com/all-contributors/all-contributors/commits?author=jfmengels" title="Documentation">📖</a> <a href="https://github.com/all-contributors/all-contributors/pulls?q=is%3Apr+reviewed-by%3Ajfmengels" title="Reviewed Pull Requests">👀</a> <a href="#tool-jfmengels" title="Tools">🔧</a></td>
    <td align="center"><a href="https://github.com/AliyevH"><img src="https://avatars.githubusercontent.com/u/5507950?v=3" width="100px;" alt=""/><br /><sub><b>Hasan Aliyev</b></sub></a><br /><a href="https://github.com/all-contributors/all-contributors/commits?author=jakebolam" title="Documentation">📖</a> <a href="#tool-jakebolam" title="Tools">🔧</a> <a href="#infra-jakebolam" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a> <a href="#maintenance-jakebolam" title="Maintenance">🚧</a> <a href="https://github.com/all-contributors/all-contributors/pulls?q=is%3Apr+reviewed-by%3Ajakebolam" title="Reviewed Pull Requests">👀</a> <a href="#question-jakebolam" title="Answering Questions">💬</a></td>
    <td align="center"><a href="https://github.com/imaskm"><img src="https://avatars.githubusercontent.com/u/20543833?v=3" width="100px;" alt=""/><br /><sub><b>Ashwani</b></sub></a><br /><a href="#design-tbenning" title="Design">🎨</a> <a href="#maintenance-tbenning" title="Maintenance">🚧</a></td>
  </tr>
  

</table>


This project follows the [all-contributors](https://allcontributors.org) specification.
Contributions of any kind are welcome!


## LICENSE

[MIT](LICENSE)