---
layout: post
title: "Snake V1 Part I: Stateless Snake Introduction"
description: "The most simple of the games, a stateless Snake, starts here."
date: 2022-08-19 18:30:00 -0000
tag: JavaScript GameDev
serie: Snake V1
---
![Snake](/images/snake1.jpg)

**Snake**, that simple game that we all had on those huge Nokia bricks fifteen years ago and we all used to kill time on is the chosen one to start this series. due to its simplicity.

It will be two parts on this first version since it will be stateless. It's simple but it gets pretty long (heh), and I want to document every single step. I want to approach this as "the tutorial I would've liked when I was learning" so if it helps someone, there is that.

1. [Description](#description)
2. [Requisites](#requisites)
3. [Index](#index)

## Description

In Snake we control a long entity that reminds us of a snake that is constantly moving inside a closed plane. If it collisions with the plane's border, we lose and have to start again. We can move the direction of the snake's head in the four basic directions (up, down, left, right) and, again, if the snake's head hits it's own body we lose.

To grow and win points there will be pieces appearing randomly in the plane that we have to "eat", thus getting longer. The more you eat, the longer you get, the more points you win. But being longer means it will be more difficult to move safely.

In this first version I'll only make the gameplay: no menus, options or extras besides the snake and the points.

## Requisites

We don't need anything to this simple version. Nothing more than basic JavaScript syntax and some understanding of JSON for data. In this case I will use basic HTML5 and jQuery for the input management. All the elements can be rendered with code so we will not need assets either. In more complex versions we could use images, sound and effects, but for now that will be out of scope. Keep it simple for now.

## Index

So with this in mind, the series will consist of three parts:

1. **Introduction**
2. [The Snake](https://ikzer.github.io/2022/08/20/snake-v1-part-ii.html)
3. [The Game](https://ikzer.github.io/2022/08/25/snake-v1-part-iii.html)
