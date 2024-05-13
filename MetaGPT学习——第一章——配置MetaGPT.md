# MetaGPT学习——第一章——配置MetaGPT

这是我第一次接触llm，API_key都是第一次接触，这里面是我遇到的一些问题，希望对以后像我这样的小白有帮助

**首先就是API的选择上**

我刚开始啥也不知道，跟着教程想要搞**openAI**的API，实在搞不来，即使用Outlook的邮箱（和微软账户绑定的，登不上可能也有关系），也没办法注册。

所幸教程还教了智普的API，顺利配置完成

首先遇到的就是

`````python
os.environ["ZHIPUAI_API_KEY"] = xxx
`````

我是在太小白了，这个“xxx”直接搞蒙我了，还好群内大佬迅速回答，只要填入智普API key就好

OK，第一步解决，接下来就是尝试运行

````python
import asyncio

from metagpt.actions import Action
from metagpt.environment import Environment
from metagpt.roles import Role
from metagpt.team import Team

action1 = Action(name="AlexSay", instruction="Express your opinion with emotion and don't repeat it")
action2 = Action(name="BobSay", instruction="Express your opinion with emotion and don't repeat it")
alex = Role(name="Alex", profile="Democratic candidate", goal="Win the election", actions=[action1], watch=[action2])
bob = Role(name="Bob", profile="Republican candidate", goal="Win the election", actions=[action2], watch=[action1])
env = Environment(desc="US election live broadcast")
team = Team(investment=10.0, env=env, roles=[alex, bob])

asyncio.run(team.run(idea="Topic: climate change. Under 80 words per message.", send_to="Alex", n_round=5))
````

立刻报错（恼）

``````报错
ModuleNotFoundError: No module named ‘pwd‘
``````

一脸懵逼的我上了CSDN，遂解决，原因是`pwd`模块只在`Linux和Unix`存在，我们需要通过`cmd`额外安装一个模块

``````cmd
pip install langchain-community==0.0.19
``````

这个我觉得用个梯子或镜像更好，不然下载很慢，导致安装失败

给个清华大学的镜像地址

``````cmd
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 这里给需要安装的模块名
``````

一开始我是在``jupyter`上运行的，所以又报了一条

```报错
RuntimeError: asyncio.run() cannot be called from a running event loop
```

这个问题是因为`jupyter`已经运行了loop，无需自个儿再调用，一个线程内只存在一个事件循环，所以如果当前线程已经有存在的事件循环了，就不应该使用 `asyncio.run `了，我选择的解决办法是将程序放在`.py`文件就好了。

遂再次运行，好的，报错

```报错
ImportError: Failed to initialize: Bad git executable.
```

这个问题是由于在你的系统中无法找到有效的git可执行文件引起的。

解决如下

```python
import os
os.environ["GIT_PYTHON_REFRESH"] = "quiet" #添加这一行
```

OK到这，程序就已经顺利跑起来了，但是。。。。。

![image-20240513193332791](C:/Users/15728/AppData/Roaming/Typora/typora-user-images/image-20240513193332791.png)

不是哥们，你咋还欠费啊，这个怎么收费的啊（悲）
