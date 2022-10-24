---
layout: post
title:  "individual log"
date:   2022-10-10 19:25:30 -0700
category: log
---

#OVERVIEW
In the present work, we were given a choice between three problems that are part of "Google Code Jam 2022" in the "Round 1A" section. The chosen problem was "Double or one thing". Google Code Jam is an international programming competition organized and managed by Google. The challenges presented by this competition are free to read, so the next section of this blog will provide the reader with a detailed introduction to the problem presented with its respective solution.

#CONTEXT
The chosen problem is called double or one thing and consists of an algorithm based mainly on the ordering and selection of character strings. The program starts working when the user gives the computer words or character strings as input. The number of strings that the code will work with will also be provided by the user in advance of the declaration of the characters. It is important to mention that each typed letter must be capitalized by the user. The statement of the problem indicates that depending on the string launched, one or more letters must be duplicated or left as is. The objective is that of all the possibilities of duplication by letter already ordered alphabetically (or lexicographically) the first option must be chosen.
The above can be illustrated with the following example, the combination of duplicates in the word "SUN" has a total of 8 possibilities:

1.- SUN 
2.- SUNN 
3.- SUUN 
4.- SUUNN 
5.- SSUN
6.- SSUNN 
7.- SSUUN 
8.- SSUUNN

After an alphabetical ordering of the sequence of previous words, we will obtain the string “SSUN” as an answer for this example, since this is the first of our already ordered list.

The result must be expressed in the following format: “Case #1: SSUN”, where the value of the number expressed after the Case# will of course vary depending on the number of cases. In addition, it must give results in less than 2 seconds.

