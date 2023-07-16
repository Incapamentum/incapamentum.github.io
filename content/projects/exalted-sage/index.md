---
title: "Exalted Sage"
description: "Documenting my progress on the development of a Discord bot as part of a DevOps project"
date: '2023-07-10'
author: "Gustavo Diaz Galeas"

usePageBundles: true

categories:
    - projects
tags:
    - DevOps
    - software engineering
series:
    - DevOps
---

According to the official Guild Wars 2 wiki, [Exalted Sages](https://wiki.guildwars2.com/wiki/Exalted_Sage) are "spellcasters found in Auric Basin. They focus and direct the vast quantities of magic which their city, Tarir, requires to keep functioning".

## Background

A game that I play on a consistent basis is [Guild Wars 2](https://www.guildwars2.com/en/). It had caught my attention primarily because I grew up watching my cousin play the original, and also because, at the time of its release, it didn't require a subscription. This was a bit novel at the time, as the leading MMOs were World of Warcraft and The Elder Scrolls Online, both of which required a monthly subscription. The game is also highly social, incorporating cooperative gameplay mechanics and supporting massive world events that all players can participate in. And, as the name implies, guilds are a big part of it.

Shortly after the COVID-19 lockdowns of 2020, I had joined a fledgling guild with an ambitious goal: to become a hub for cooperation and organize routine events. When I had joined, I believe I was the second non-office member. Now, after three years, I am now an officer of the guild. However, my main task is much more technical than that of the other officers: I maintain a Discord bot for our server.

The project first started out simple: it was written in Python, making use of the [Discord.py](https://discordpy.readthedocs.io/en/stable/) API. It wasn't anything novel or groundbreaking, and it was my first foray into actually being the lead developer and maintainer for a project. However, as time went on, the server grew, the guild grew, and making use of Python was becoming more and more cumbersome due to the language being weakly typed. So I had to make a decision: either keep piling up the technical debt of having to use Python, or switch over to a compiled (and strongly typed!) language.

The latter choice was selected, and in early 2022 I had undertaken a massive bottom-up rewrite from Python to C#, making use of the [Discord.NET](https://discordnet.dev/guides/introduction/intro.html) library. I had also started to look into CI/CD, and inadvertently transformed the project to a DevOps one.

For full clarity: at the time I have heard the term DevOps, but never once really understood what it meant or what it actually was and what it entailed. It wasn't until a conversation with a friend, whom is my goto guy if I ever have any questions on the subject, that I realized the whole project rewrite most definitely constitutes a DevOps project.

## Purpose

As the project is still in active development (you can actually check out the repo right [here](https://github.com/Incapamentum/Exalted-Sage)), things often pop up that I get to work on. Sometimes they can be quick fixes, however very rarely it can take me up to a year to even figure out what's going on.

Having pursued this project for this long, it's allowed me an opportunity to develop new skills and further refine existing ones. More importantly, it's given me experience in troubleshooting and learning new dev practices. This project will support a series of psots that will discuss some of the things I encountered and how I approached solving them.

## AI Disclosure

I've started to make use of OpenAI's ChatGPT to help me solve some problems that I've encountered. I don't fully copy all the code that it generates, but I do take inspiration from it. Some posts will include how I integrate the use of LLMs to my workflow in general.