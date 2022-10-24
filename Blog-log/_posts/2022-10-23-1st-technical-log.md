---
layout: post
title:  "1st technical log"
date:   2022-10-23 19:25:30 -0700
category: tech log
---

<h2 style = text-align: center>OVERVIEW</h2>
 
In this work, we were given a choice between three problems that are part of "Google Code Jam 2022" in the "Round 1A" section. The chosen problem was "Double or one thing". Google Code Jam is an international programming competition organized and administered by Google. The challenges presented by this competition are free reading, so the next section of this blog will provide the reader with a detailed introduction to the problem presented with its respective solution.
 
<h2>`CONTEXT`</h2>
 
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

`SOLUTION`

The solution consists in taking the number of cases that will be obtained (in this way, we will know how many words or strings of characters will be evaluated.) and obtaining the word so that the program performs the necessary operations and print the result in the format already indicated. Next, we evaluate character by character and check if the next character we are evaluating is greater than or less than. From here, there are three possibilities. If the current character is less than the next then there will be duplication, on the other hand, if the current character is the same as the next then the evaluated character will now be the next. In the case where the character is older then there will be no duplication.
 
The above was an overview of the solution to the problem, now I will explain the solution more specifically based on the code.
 
We will call “T” the number of cases. We start a "for" loop where we declare four variables: "word", "pair", "counter" and "folded" iterating "T" times in total.

{% highlight ruby %}

T = int(input())

for i in range(T):

    word = ""
    couple = []
    counter = 1
    doubled = ""

    word = (str(input()))

{% endhighlight %}

We have that "word" is a string type variable that will be used to store the input with the character string to be evaluated, "couple" is a list which will contain a list with two elements, the first will contain a character from the word variable, while the second element will store the number of repetitions in a row.
The variable “counter” will be used to count consecutive letters that are repeated (which are stored in the list couple) and is initialized to 1. To store the result we will use “doubled”.

{% highlight ruby %}

    for j in range(len(word) - 1):
        if word[j] == word[j + 1]:
            counter += 1
        else:
            couple.append([word[j], counter])
            counter = 1
    couple.append([word[-1], counter])

{% endhighlight %}

Now we will make use of another “for” loop that will help us go through each hidden letter in "word" without considering the last letter. Inside the loop, we place a conditional so that the counter increases by 1 when the evaluated letter is equal to the next one. We can see that a list with the format [letter, times repeated] is added inside "couple" and the counter is reset to 1. Outside the loop, the list is added again to "couple" the last letter of "word" with the number of times repeated.
 
For example, if the word is "COUNTER", the "even" list will have the form [[B, 1], [O, 2], [K, 2], [E, 2], [P, 1], [E, 1], [R, 1]].
 
The next and last “for” cycle of the code consider the dimension of the “couple” variable and 1 is subtracted from it so as not to add or modify the final letter. The reason for this is that alphabetically speaking, the evaluation scrolls to the right (which is not convenient), plus this prevents the next comparison from going outside the bounds of the list dimension, where it scrolls from the first element up to the penultimate of “couple”.

{% highlight ruby %}

    for j in range(len(couple) -1):
        if couple[j][0] < couple[j +1][0]:
            doubled = doubled + 2 * couple[j][1] * couple[j][0]
        else:
            doubled = doubled + couple[j][1] * couple[j][0]
    doubled = doubled + couple[-1][1] * couple[-1][0]
    #print(doubled)f
    print(f"Case #{i + 1}: {doubled}")

{% endhighlight %}

The "if" statement compares the letter of the array containing the letter and its repetition number with the next array. If alphabetically the current letter goes first then we multiply by 2 the number of times that determined letter and in turn, the result of the product will be multiplied by the letter stored in the "doubled" variable. If the letter within "pair" is greater than the next, that same letter multiplied by the repeated times is added to "duplicate".
Outside the loop, we add the last letter and its repetitions. Finally, the result is printed in the format specified above.

`ALTERNATIVE SOLUTIONS`

An alternative procedure may be to create a list of all possibilities and use python's "min()" function (which returns the minimum value of any dataset automatically), however, although the procedure works at runtime is greater and if the word exceeds a certain number of characters, it is impossible to obtain the result in less than 2 seconds.
