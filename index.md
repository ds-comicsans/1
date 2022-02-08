`про киллер#2413`

# Как сделать такие же комманды как у богдана

## 1. Смена префикса

Самое сложное в моём боте, это смена префикса. Я решил обсудить это сразу же.

**Код:**

```
@bot.command(aliases=["префикс"])
@commands.has_permissions(administrator=True)
async def прификс(ctx, prefix):
    bot.command_prefix = prefix
    await ctx.send(f"📝 Прификс изменон на `{prefix}`")
```

Казалось-бы, выглядит как будто это не сработает, но кто не знал, когда переставить местами `prefix` и `bot.command_prefix`, они работают по другому.

### Как добавить эту фишку со сменой префикса в комманду help

В комманде help, после `@bot.command()` и `async def ...(ctx):` на уровне с остальным кодом пишем:

```
prefix = bot.command_prefix
```

Далее, где применяется префикс, просто пишем `{prefix}`.

## 2. Самое лёгкое

Самое лёгкое, это `time.sleep()`, он использовался в комманде 'б.кийс'

> Для того, чтобы `time.sleep` работал, нужно обратится к `import time`.

Вот пример использования:

```
@bot.command()
async def ...(ctx): 
    await ctx.send('Подождите 3 секунды', delete_after=1)
    time.sleep(3)
    embed = discord.Embed(title="Готов!", description='Вы были обречены! ххаххаха', color=0xcd19ff)
    await ctx.send(embed=embed)
```

**Действие:**

```
= Бот отправляет сообщение: =
Подождите 3 секунды
= Через 3 секунды отправляется embed: =
| **Готов!**
| Вы были обречены! ххаххаха
= И сразу после embed удаляется первое сообщение =
```

## 3. Чат-бот

Функции чат-бота. Очень легкие.

**Пример 1**

### Как сделать так, чтобы бот реагировал на "привет" и на "ПРИвЕт":

```
@bot.event
async def on_message(message):
    greetings = ["привет" , "ку" , "Привет" , "Здарова" , "Здравствуйте" , "Добрый день" , "Доброе утро" , "доброе утро"]
    if any(word in message.content.lower().split() for word in greetings):
        if message.author == bot.user: return
        await message.reply(f"{random.choice(hi)}")
```

Бот смотрит, не содержит ли любое сообщение `greetings`, если содержит, то он отправляет рандомный ответ.

> Бот выберает рандомный ответ из:

```
hi = ['Привет, участник!', 'Привет!']
```

### Как сделать так, чтобы бот реагировал на пинг:

```
    if bot.user.mentioned_in(message):
        await message.reply(f"{random.choice(aaa)}")
```

Бот смотрит, не пинганул ли кто его. Способы пинга: ответ с включенным упоминанием, @everyone и @here, пинг бота (@bot).

Бот выберает рандомный ответ из 'aaa'

```
aaa = ['Привет, мой префикс {ваш префикс}!', 'С какой целью вы меня пинганули, сударь?']
```

## 4. Бот присоеденился, отправил сообщение в нулевой канал.

Нулевой канал это тот канал, который находится первым. Боту не важно, имеет ли он доступ к ниму, либо нет.

Пример кода:

```
@bot.event
async def on_guild_join(guild):
    await guild.text_channels[0].send('''
Приветствую!
''')
```

## 5. Статус бота.

### 1. Сам статус:

**Онлайн** - `online`
**Отошёл** (я не знаю как там по русски, потому-что у меня дс английский) - `idle`
**Не беспокоить** - `do_not_disturb`

### 2. Играет/смотрит/стримит и т.д.

**Играет** - `discord.Game`
**Смотрит** - `discord.Watching`
**Стримит** (см. в интернете подробности0 - `discord.Live`

**Пример кода:**

```
@bot.event
async def on_ready():
    activity = discord.Game(name="оснавнои прификс: б.", type=3) # изменить актив можно после activity = discord.(ЧТОДЕЛАЕТБОТ)
    await bot.change_presence(status=discord.Status.do_not_disturb, activity=activity) # discord.Status.(СТАТУС)
```

## 6. Бот присылает сообщение, когда комманда не найдена.

**Пример кода:**

```
@bot.event
async def on_command_error(ctx, error):
    if isinstance(error, commands.CommandNotFound ):
        await ctx.send(embed = discord.Embed(description = f'😥 **{ctx.author.name}**, я ни нашол каманду такую блен', color=0xcd19ff))
```

**Пример:** вы ввели !aaa, а 'aaa' не является коммандой, бот выдаёт сообщение ^, потому что нет такой комманды.


***

**Спасибо что прочитал**
