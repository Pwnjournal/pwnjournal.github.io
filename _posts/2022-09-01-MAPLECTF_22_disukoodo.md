---
layout: post
title:  MAPLECTF_22_disukoodo
image: /assets/img/UNIBUC_RE.jpg
date:   2022-09-01 16:39:01
tags:
- pwn
- discord bot
description: Discord bot hacking privilege escalation
categories:
- maplectf 22
---

Discord bot hacking privilege escalation

I just found this discord bot that one of our rivaling team's members made, and I'm pretty sure they are using this in their own team discords - can you get the names of the servers that they are in with it, just in case? This might reveal a Mysterious Mega Merger team in the making after all 😉

Note: please ensure discord.py is < 2.0.0 if you want to run the bot locally.


As we are given the bot code we can get the vulnerabilities from there

The interesting functions are:

```
@bot.command(description='BOT MODS ONLY - manually sets a member as a premium member')
@commands.check(is_privileged)
async def registerprem(ctx, member: User):
    prem_users.append(member.id)
    await ctx.send('Manually added <@!' + str(member.id) + '> as a premium member!')

@bot.command(description='Echos a given message! Premium members gets a special secret feature ;)')
async def echo(ctx, *, msg):
    if len(msg) > 1990:
        await ctx.send('Sorry, your message to be echoed is too long!')
    else:
        val = msg.split(' ')[0]
        to_echo = (msg[len(val)+1:] * min(int(val), 100)) if await is_prem(ctx) and val.isdigit() else msg
        await ctx.send('`' + utils.escape_markdown(to_echo) + '`')

```

We can register as a premium user if we can somehow trick the bot to do the command itself.

For the registering command we only need our user ID:

![](/assets/img/2022-08-31-08-06-50.png)


Next we need to get the servers this bot has been into so we need to crash it. We use the new priviledges we got in order to output more than the maximum amount.


![](/assets/img/2022-08-31-08-08-34.png)


We then search the ids to get the servers:

![](/assets/img/2022-08-31-08-09-23.png)