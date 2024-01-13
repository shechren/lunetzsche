---
title: python - discord - 369 게임
date: 2022-07-04 13:27:52 +/-09:00
category: [python/discord]
tag: [python, discord]
---

```python
import asyncio
import discord
from discord.ext import commands
```

필요 모듈 임포트. 참고로 discord 모듈은 필요하지 않다.

```python
class ThreeSixNine(commands.Cog):
    def __init__(self, bot):
        self.bot = bot
        self.count = 1

def setup(bot):
    bot.add_cog(ThreeSixNine(bot))
```

이제 저 줄 사이에 코드를 입력하면 된다.

```python
@commands.Cog.listener()
async def on_message(self, message):
    if message.author.bot:
        return
    msg = message.content
    if msg.startswith("369 시작"):
        self.count = 1
        await message.channel.send("369 시작.")
        await ThreeSixNine.gamePlay(self, message)
```

369 시작 명령어를 받아온다. 카운트는 1로 초기화된다.

```python
async def gamePlay(self, message):
    try:
        c = await self.bot.wait_for("message", timeout=5)
    except asyncio.TimeoutError:
        await message.channel.send("시간 초과! 게임 종료!")  # 오류
    else:
        await ThreeSixNine.func(self, c, message)
```

5초가 지나면 타임아웃이다. 만약 타임아웃에 걸리지 않는다면 else를 실행한다.

```python
async def func(self, c, message):
    msg = c.content
    if c.author.bot:
        return
    elif "3" in msg or "6" in msg or "9" in msg:
        await message.channel.send("게임 오버!")  # 369를 입력하면 게임 오버
    elif str(self.count) != msg:
        if msg == "짝":
            if "3" in str(self.count) or "6" == str(self.count) or "9" == str(self.count):  # 369에 짝을 입력했을 경우
                self.count += 1
                await message.channel.send(f"입력한 단어: {msg}")
                await ThreeSixNine.gamePlay(self, message)
            else:
                await message.channel.send("게임 오버!")
        else:
            await message.channel.send("게임 오버!")
    else:
        self.count += 1
        await message.channel.send(f"입력한 숫자: {msg}")
        await ThreeSixNine.gamePlay(self, message)
```

첫째, 3이나 6이나 9가 메세지에 입력되면 자동으로 게임 오버가 된다.

둘째, 카운트가 3이나 6이나 9일 때 "짝"을 입력하면 카운트를 추가하고 게임을 진행시킨다.

셋째, 그것이 아닐 경우 게임을 종료시킨다.

마지막으로 그냥 일반 숫자들을 입력할 경우 카운트를 추가하고 게임을 진행시킨다.

---