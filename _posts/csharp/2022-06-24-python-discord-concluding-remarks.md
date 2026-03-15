---
title: python - discord - 끝말잇기 만들기
date: 2022-06-27 10:32:21 +/-09:00
category: [python/discord]
tag: [python, discord]
---

```python
import asyncio
import discord
from discord.ext import commands
```
필요 모듈을 가져온다. 디스코드봇의 다른 파일에 넣을 거니까 클래스로 제작할 것이다.

@bot.event는 @commands.Cog.listener(), @bot.command()는 @commands.command로 데코레이터가 치환되어야 작동함을 잊지 말자.

```python
class LastGame(commands.Cog):
    def __init__(self, bot):
        self.bot = bot
        self.check_string = [""]
        self.check_word = [""]
        self.count = 0
        
    @commands.Cog.listener()
    async def on_message(self, message):
        if message.author.bot:
            return
        msg = message.content
        if msg.startswith("끝말잇기"):
            self.check_string = [""]
            self.check_word = [""]
            self.count = 0
            await message.channel.send("끝말잇기 시작.")
            await LastGame.play(self, message)
```
기본적인 명령을 만든다. 만약 봇이 채팅했다면 이를 무시한다.

check_string. check_word. count 이 3개에 대해서는 이따 조건문에서 쓸 것이다.

---

끝말잇기가 시작되고, play 함수로 일을 넘긴다.

```python
async def play(self, message):
    try:
        c = await self.bot.wait_for("message", timeout=5)
    except asyncio.TimeoutError:
        await message.channel.send("시간 초과! 게임 종료!") # 오류
    else:
        if c.author.bot:
            return
        last_word = c.content[-1]
        first_word = c.content[0]
        if c.content in self.check_string:
            await message.channel.send("이미 사용된 문장입니다.")
        elif first_word != self.check_word[-1] and self.count != 0:
            await message.channel.send("끝말이 아닙니다. 게임 종료!")
        elif c.content[0] == c.content[-1]:
            await message.channel.send(f"중복된 단어입니다. 게임 종료!")
        else:
            self.check_string.append(c.content)
            self.check_word.append(last_word)
            self.count += 1
            await message.channel.send(f"마지막 글자는 {last_word}, {self.count}번째!")
            await LastGame.play(self, message)

def setup(bot):
    bot.add_cog(LastGame(bot))
```

play 함수에서는 본격적으로 끝말잇기 게임을 다룬다.

코루틴 wait_for 메소드를 이용해 응답을 받는다.

c는 메세지이며, 5초를 카운트한다. 5초가 지나면 타임아웃으로 except가 실행되며, 5초 안에 응답하면 else가 실행된다.



else문에서는 역시나 제일 먼저 봇의 응답을 무시한다.

last_word는 마지막 글자를 추출한다.

first_word는 첫 번째 글자를 추출한다.

문장이 check_string 안에 있다면 이미 사용된 문장이므로 게임을 종료시킨다.

첫 글자가 이전 대화의 마지막 글자와 다르고 + 1회 이상 대화가 이어질 경우 끝말이 아니므로 게임을 종료한다.

첫 글자가 마지막 글자와 일치할 경우 계속 순회하므로 중복 단어 처리로 게임을 종료한다.



게임 실행 부분은 다음과 같다.

중복 문장 처리를 위해 메시지를 check.append에 추가한다. 이는 매 게임 시작 시 on_message() 함수에서 초기화한다.

마지막 단어를 check_word에 추가한다. 이는 매 게임 시작 시 on_message() 함수에서 초기화한다.

count를 회마다 1씩 늘린다. 이는 매 게임 시작 시 on_message() 함수에서 초기화한다.

---