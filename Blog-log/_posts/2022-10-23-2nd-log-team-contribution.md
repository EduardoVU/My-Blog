---
layout: post
title:  "My contribution to the team"
date:   2022-10-23 19:25:30 -0700
category: tech log
---

<h2>OVERVIEW</h2> 
 
In the following work, we were asked to solve the problems found in "Qualification round" and "Round 1A" and solve them in 4 different programming languages: python, kotlin, dart, and typescript. In this document, I will talk about the programs in which I was involved.
Google Code Jam is an international programming competition organized and administered by Google. The challenges presented by this competition are free reading, so the next section of this blog will provide the reader with a detailed introduction to the problems presented with their respective solution.

<h2>CONTEXT – 3D printers</h2>

You were given three printers and will use each one to print one of the D's. All printers use ink from 4 individual cartridges of different colors (cyan, magenta, yellow, and black) to form any color. For these printers, a color is uniquely defined by 4 non-negative integers c, m, y, and k, which indicate the number of ink units of cyan, magenta, yellow, and black ink (respectively) needed to make the color. 
The total amount of ink needed to print a single D is exactly 1 000 000 units. For example, printing a D in pure yellow would use 1 000 000 units of yellow ink and 0 from all others. Printing a D in the Code Jam red uses 0 units of cyan ink, 500000 units of magenta ink, 450000 units of yellow ink, and 50000 units of black ink 
 
A simple solution to this problem is getting the minimum value of each color of the 4 printers. Once we get the minimum values, we add them and if they add up to one million exactly, we can print. If the result is less than 1 000 000, we cannot print anything. And if the result is greater than one million, you must subtract units of ink to get to one million. Now, how do we know from which cartridge of ink you should subtract? Easy, from the 4 minimum values, you get the maximum value, like this:  Crt = max(c,m,y,k). To that cartridge, you subtract until all 4 values add up to one million, or if that value is equal to zero. If this is the case, that value stays zero, and you subtract from the next maximum value. That process should go on until you get the sum of one million. 

<h2>SOLUTION</h2>

A simple way to solve this problem is to select the minimum amount of each color of the three printers, if the sum of the four colors is less than 10^6, the result is "IMPOSSIBLE".
If the sum is equal to 10^6, that would be the amount of ink in response, instead, if it is greater than 10^6, the ideal would be to reduce one of the 4 colors to 0 and make comparisons again, only considering those new 3 colors, if the sum is greater than 10^6, it continues to do the same thing, reduce the current color to 0 and compare until it is less than 10^6. When the sum value is less than 10^6, we can reduce the current color value to 10^6 minus the sum value, so this would give us the exact result.

It is necesarly to import "dart:io"

{% highlight ruby %}

void main() {
  int cases = int.parse(stdin.readLineSync());
  List<dynamic> results = List.filled(cases, 0);
  List<List<int>> caseArray = [[], [], []];

  for (int i = 0; i < cases; i++) {
    for (int j = 0; j < 3; j++) {
      dynamic temporal = stdin.readLineSync().split(" ");

      int c = int.parse(temporal[0]);
      int m = int.parse(temporal[1]);
      int y = int.parse(temporal[2]);
      int k = int.parse(temporal[3]);

      caseArray[j] = [c, m, y, k];
    }
    results[i] = calculateInk(caseArray);
  }

{% endhighlight %}

In this part of the code, the number of cases to be tested is taken as input, and a dimension list is created for the number of cases and filled with “0” values. Then a list with three empty lists inside is created. The first “for loop” is to store the results in the "results" list. The second “for loop” stores the amount of ink for each color in one of the empty lists of the variable “caseArray”. Then it does the “calculateInk” function with the values stored in “caseArray”.

{% highlight ruby %}

import 'dart:io';

dynamic calculateInk(List<List<int>> arr) {
  List<List<int>> printers = [arr[0], arr[1], arr[2]];
  int TOTAL = 1000000;
  int curr = 0;
  List<int> currPrint = [0, 0, 0, 0];

  for (int i = 0; i < 3; i++) {
    curr = printers[i].reduce((value, element) => value + element);
    if (curr < TOTAL) {
      return "IMPOSSIBLE";
    }
  }

  List<int> minInk = printers[0];
  for (int i = 0; i < 4; i++) {
    for (int j = 0; j < 3; j++) {
      if (minInk[i] > printers[j][i]) {
        minInk[i] = printers[j][i];
      }
    }
  }

  {% endhighlight %}

After declaring variables, where “printers” stores the amounts of ink that each printer has, a for loop is used to add the ink values and store them in the variable “currPrint” and the comparison is made, if it is less than 10^ 6 returns as a result "IMPOSSIBLE".
Continuing, in the minInk variable of the first printer, those “for loops” are so that the smallest amount of ink of each color of the first 3 printers is stored.


{% highlight ruby %}

  curr = minInk.reduce((value, element) => value + element);
  if (curr < 1000000) {
    return "IMPOSSIBLE";
  } else {
    int ink = 0;

    for (int i = 0; i < 4; i++) {
      if (ink + minInk[i] >= 1000000) {
        currPrint[i] = minInk[i] - (minInk[i] + ink - 1000000).abs();
        break;
      } else {
        ink = ink + minInk[i];
        currPrint[i] = minInk[i];
      }
    }
  }
  return currPrint;
}

{% endhighlight %}

In the next section, the sum is made and the comparison is made again, if it is less than 10^6, the result is "IMPOSSIBLE". Later, if the sum of the remaining inks is greater than or equal to 10^6, the subtraction of 10^6 minus the current sum, and that value is subtracted from the surplus, so the answer would already be stored in the "currPrint" list, on the other hand, if the sum is less than 10^6, it reduces the current ink evaluated and continues the cycle until the first condition is met.
 
<h2>ALTERNATIVE SOLUTIONS</h2> 
 
We could reduce or increase the amount of each color one by one, however, this would make the program too slow, so it would not meet the execution time.

<h2>CONTEXT – Twisty little passages</h2>

You are investigating a cave, which is a simple undirected graph with N vertices and no isolated vertices. At the start, you are told that the cave has N <= 10^5 rooms. The vertex number you are at and the number of incident edges.  
When in a room, you can identify what room you are in and see how many passages it connects to, but you cannot distinguish the passages. You want to estimate the number of passages that exist in the cave. You are allowed to do up to K = 8000 operations. An operation is either: 
Teleport: You choose to teleport to a vertex number of your choice 
Walk: You choose to be moved to a uniformly chosen random neighbor of the current vertex. 
After each move, you are told the vertex number and the number of incident edges. With this information, you have to estimate the number of edges in the graph in an approximation error of 33.3%, and you are to succeed in at least 90% of the test cases. 

<h2>SOLUTION</h2>

Strategically we use that 50% will walk through the passages, while the other 50% will teleport.
In this way, we can calculate an average of the passages teleported, multiply it by the number of passages seen, add to the passages walked, and divide it by two, since each passage joins two rooms.
We will take as N: the number of rooms, K as the number of attempts to solve the case, R: as the current room, and P number of passages seen from the room.

It is necesarly to import "dart:io"
 

{% highlight ruby %}

void main() {
  int cases = int.parse(stdin.readLineSync());

  for (int i = 0; i < cases; i++) {
    solve();
  }
}

{% endhighlight %}

In this section, the number of cases is taken as input, which indicates how many times the code will be repeated and the number of results that are expected.


{% highlight ruby %}

import 'dart:io';

dynamic solve() {
  String temporal = stdin.readLineSync();
  List splitted = temporal.split(' ');

  int N = int.parse(splitted[0]);
  int K = int.parse(splitted[1]);

  String otemporal = stdin.readLineSync();
  List osplitted = otemporal.split(' ');

  int R = int.parse(osplitted[0]);
  int P = int.parse(osplitted[1]);

  Set<int> roomsLeft = {};

  for (int i = 1; i < N + 1; i++) {
    roomsLeft.add(i);
  }

  if (roomsLeft.contains(R)) {
    roomsLeft.remove(R);
  }

  {% endhighlight %}

  In this section, the values of N, K, R, and P are taken. In addition, the set is created with the room numbers and the room in which it starts is removed from the set.

{% highlight ruby %}

  int degree = P;
  num degreeRl = degree;
  num countR1 = 1;
  int iterCounter = 0;

  for (int i = 0; i < K; i++) {
    if (i % 2 == 0) {
      print("W");
      otemporal = stdin.readLineSync()!;
      osplitted = otemporal.split(' ');

      R = int.parse(osplitted[0]);
      P = int.parse(osplitted[1]);
    } else {
      print("T ${roomsLeft.elementAt(iterCounter)}");
      otemporal = stdin.readLineSync()!;
      osplitted = otemporal.split(' ');

      R = int.parse(osplitted[0]);
      P = int.parse(osplitted[1]);

      degreeRl += P;
      countR1 += 1;
      iterCounter += 1;
    }
    if (roomsLeft.contains(R)) {
      roomsLeft.remove(R);
      degree += P;
    }
  }

  num degreeAvg = degreeRl / countR1;
  num result = (degree + degreeAvg * roomsLeft.length) / 2;
  print("E ${result.round()}");
}

{% endhighlight %}

Variables are declared that will be used to calculate the average of the passages seen with the teleports, in addition, a counter is used to iterate with the next room. It is also shown how 50% walk (using even numbers) while the other 50% teleport (using odd numbers). In addition, the rooms they were in are removed from “roomsLeft”. Finally, the average is calculated, multiplied by the rooms that were not visited, and added to the passages seen, finally dividing by two and calculating an approximate of the cave passages.


<h2>ALTERNATIVE SOLUTIONS</h2> 
  
I tried to consider a way in which I could make an estimate only by teleporting, although I didn't know how to implement it.


<h2>CONTEXT – Double or one thingz</h2>

You are given a string of uppercase English letters. You can highlight any number of the letters (possibly all or none of them). The highlighted letters do not need to be consecutive. Then, a new string is produced by processing the letters from left to right: non-highlighted letters are appended once to the new string, while highlighted letters are appended twice. 
 
Given a string, multiple strings can be obtained as a result of this process, depending on the highlighting choices. Among all of those strings, output the one that appears first in alphabetical (also known as lexicographical) order. 
 
One of the approaches that were found was to go analyzing letter by letter and compare it to the next one with each iteration. If a letter was the same as the next one, it would only increase by 1 in a variable that counted how many times a letter was repeated consecutively. Once a letter was different than the next one, it would compare if it was, alphabetically, greater or less than the next one. If it was greater than, it would only append the single letter multiplied by the number of times it was consecutively repeated (being 1 as the default) to the final answer (a string), and if the next letter was not greater than the one it was at, it would append the letter multiplied by two multiplied by the number of times it was consecutively repeated. In the end, we would have the word that appears first in alphabetical order among all possible combinations.  


 
<h2>SOLUTION</h2>


The proposed solution consists of taking the number of cases that will be obtained (in this way, we will know how many words or character strings will be evaluated) and obtaining the word so that the program performs the necessary operations and print the result in the format already indicated. Next, we evaluate character by character and check if the next character we are evaluating is greater than or less than. From here, there are three possibilities. If the current character is less than the next then there will be duplication, on the other hand, if the current character is the same as the next then the evaluated character will now be the next. In the case where the character is larger then there will be no duplication.
 
The above was an overview of the solution to the problem, now I will explain the solution more specifically based on the code.
We will call “T” the number of cases. We start a "for" loop where we declare four variables: "word", "couple", "counter" and "doubled" iterating "T" times in total.

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

Now we will make use of another “for” loop that helps us go through each letter stored in "word" without considering the last letter. Inside the loop, we place a conditional so that the counter increases by 1 when the evaluated letter is equal to the next one. We can see that a list with the format [letter, times repeated] is added inside "couple" and the counter is reset to 1. Outside the loop, the list is added again to "couple" the last letter of "word" with the number of times repeated.
 
For example, if the word is "BOOKKEEPER", the list "couple" would have the form [[B, 1], [O, 2], [K, 2], [E,2], [P, 1], [E, 1], [R, 1]].
 
The next and last “for” cycle of the code consider the dimension of the “couple” variable and 1 is subtracted from it so as not to add or modify the final letter. The reason for this is that alphabetically speaking, the evaluation scrolls to the right (which is not convenient), plus this prevents the next comparison from going outside the limits of the dimension of the list, where it scrolls from the first element until the penultimate of “couple”.

{% highlight ruby %}

    for j in range(len(couple) -1):
        if couple[j][0] < couple[j +1][0]:
            doubled = doubled + 2 * couple[j][1] * couple[j][0]
        else:
            doubled = doubled + couple[j][1] * couple[j][0]
    doubled = doubled + couple[-1][1] * couple[-1][0]

    print(f"Case #{i + 1}: {doubled}")

{% endhighlight %}

The "if" statement compares the letter of the array containing the letter and its repetition number with the next array. If alphabetically the current letter goes first then we multiply by 2 the number of times that determined letter and in turn, the result of the product will be multiplied by the letter stored in the "doubled" variable. If the letter within "pair" is greater than the next, that same letter multiplied by the repeated times is added to "duplicate".
Outside the loop, we add the last letter and its repetitions. Finally, the result is printed in the format specified above.
 
 
<h2>ALTERNATIVE SOLUTIONS</h2>

An alternative procedure may be to create a list of all possibilities and use python's "min()" function (which returns the minimum value of any dataset automatically), however, although the procedure works at runtime is greater and if the word exceeds a certain number of characters, it is impossible to obtain the result in less than 2 seconds.


<h2>CONTEXT – Equal sum</h2>
You are given a set of distinct integers. You need to separate them into two non-empty subsets such that each element belongs to exactly one of them and the sum of all elements of each subset is the same. 
An anonymous tip told us that the problem above was unlikely to be solved in polynomial time (or something like that), so we decided to change it. Now you get to decide what half of the integers are! 
This is an interactive problem with three phases. In phase 1, you choose N distinct integers. In phase 2, you are given other N integers that are distinct from each other and from the ones you chose in phase 1. In phase 3, you have to partition those 2N integers into two subsets, both of which sum to the same amount. All 2N integers are to be between 1 and 10^9, inclusive, and it is guaranteed that they sum up to an even number. 
 
 
SOLUTION 
Strategically, powers of 2 are used, since the next one is twice the one selected, in this way it is optimized locally.
For this problem we need o import import 'dart:io' and import 'dart:math';


{% highlight ruby %}

void main() {
  int T = int.parse(stdin.readLineSync()!);

  for (int i = 0; i < T; i++) {
    int N = int.parse(stdin.readLineSync()!);

    List<num> A = powersOftwo(N);

    List<String> temporal = A.map((e) => e.toString()).toList();
    print(temporal.join(" "));

    List<String> bString = stdin.readLineSync()!.split(" ");

    List<int> B = bString.map(int.parse).toList();

    List<num> C = solveSum(A, B, N);

    List<String> listC = C.map((e) => e.toString()).toList();
    print(listC.join(" "));
  }
}

{% endhighlight %}

The number of cases, or solutions, is taken as the variable T. N is the size or number of elements that the entries will have. The value of the inputs continues to be declared and proceeds to filter to convert them into variables of type int, in addition, the poweroftwo function is used, which it creates will be explained later. The solveSum() function is also used, which will be explained later. Finally, it prints the results obtained from both functions.


{% highlight ruby %}


import 'dart:io';
import 'dart:math';

List<num> powersOftwo(N) {
  List<num> A = [];

  for (int i = 0; i < 30; i++) {
    A.add(pow(2, i));
  }

  if (N <= 30) {
    return A;
  } else {
    for (int i = 0; i < N - 30; i++) {
      A.add(pow(10, 9) - i);
    }
    return A;
  }
}

{% endhighlight %}

The powerOftwo function creates a list with the powers of 2, from 2^0 to 2^29, if N is greater than 30, it is added to the list with the powers of 2, values less than one by one to 10^9, depending on the difference of N - 30.


{% highlight ruby %}

solveSum(A, B, N) {
  List<int> equalSum = [];
  for (int i = 0; i < B.length; i++) {
    A.add(B[i]);
  }
  A.sort();
  num aSum = 0;
  num bSum = 0;

  for (int i = A.length - 1; i >= 0; i--) {
    if (aSum > bSum) {
      equalSum.add(A[i]);
      bSum += A[i];
    } else {
      aSum += A[i];
    }
  }
  return equalSum;
}

{% endhighlight %}

This section is used to try to minimize the differences between both sets, joining both sets, ordering them, and making sure that the difference between both sets is 0.
 
<h2>ALTERNATIVE SOLUTIONS</h2> 

Something else that I considered, is to add both sets and divide them by 2, in this way, by properties of subsets, I know that one subset adding their values, would result in half of the sum of all the elements of the complete set. You could iterate through each element of each set until you get half the sum of all the elements in the set, however, this would be too slow a process. Perhaps with dynamic programming, it can be optimized.

<h2>CONTEXT – Weightlifting</h2>

On this problem, you must count as an operation each time you either put on or off weight to complete each exercise. As inputs you are given T number of cases on the first line, on the following line will be 2 integers with E number of exercises and W types of weights. Then, will follow E numbers of lines, with each one containing the weights required for each exercise. At the end of each line E of exercises, you must empty the stack of weights.  
For example, if you have 3 exercises (e) with 1 weight type (w), and each of the exercises needs 1, 2, and 1 as weights, the result will be 4 operations. 
One operation to put on a 1w 
One operation to put another 1w to complete an exercise of 2w 
One operation to put off 1w and complete an exercise of 1w 
One operation to put off 1w and empty the stack  
 
The number of exercises and weights are always different, nevertheless, to achieve an optimal way of loading and unloading weights, it’s imperative to find a set of common weights that never left the stock. In other words that set of weights must belong to the intersection of all the exercise requirements. 
Then, we should define a differential (dp) as an optimal way to do the movements through all exercises. Keep in mind that the starting and finishing point must be the same; out of the common set of weights. Even though the possible movements are less than before, there are several options, so what we should do it’s to calculate the shorter one. 

<h2>SOLUTION</h2> 
 
In this problem, the optimal arrangement of the weights must be sought, in such a way that the one that is used the most is placed at the bottom of the stack, and thus find the optimal way to avoid making the greatest number of movements.
We need to import "dart:io".

{% highlight ruby %}

void main() {
  num INF = (999999999999999999);

  int cases = int.parse(stdin.readLineSync());

  List resultsArray = [];

  for (int i = 0; i < cases; i++) {
    resultsArray.add(solve(INF));
  }
  for (int i = 0; i < cases; i++) {
    print("Case #${i + 1}: ${resultsArray[i]}");
  }
}

{% endhighlight %}

In this section, a variable is declared that simulates to be an infinitely large value, and a number stored in "cases" is taken as input, which indicates how many times the program will be repeated and the number of expected results. An empty array is created and a for loop is made which calls a function called "solve()", the result of that function will be stored in the aforementioned array. Finally, the results are printed.


{% highlight ruby %}

import 'dart:io';

dynamic solve(INF) {
  dynamic temporal = stdin.readLineSync()!.split(" ");
  int e = int.parse(temporal[0]);
  int w = int.parse(temporal[1]);
  List<dynamic> allExcercises = [];
  List<dynamic> dp = [];

  for (int i = 0; i < e; i++) {
    allExcercises.add(stdin.readLineSync()!.split(" ").map(int.parse).toList());
  }

  for (int i = 0; i < e; i++) {
    dp.add([]);
    for (int j = 0; j < e; j++) {
      dp[i].add(0);
    }
  }

  List<dynamic> current = [];

  for (int i = 0; i < e; i++) {
    current = [];
    for (int l = 0; l < w; l++) {
      current.add(INF);
    }
    for (int j = i; j < e; j++) {
      for (int k = 0; k < w; k++) {
        if (allExcercises[j][k] < current[k]) {
          current[k] = allExcercises[j][k];
        }
      }
      dp[i][j] = current.reduce((value, element) => value + element);
    }
  }

{% endhighlight %}

The solve() function receives an INF parameter, which is the variable where the number that simulates being an infinite value is stored. Then you receive as input two numbers, the number of exercises you should do and the number of weights you should use. Two empty lists are created, allExercises and dp. A cycle is used to store the sequence of the exercises with their respective weights, different methods are used so that the input passes from string to int and is stored in a list.
Then a loop is used to store in the variable dp the number of exercises in lists, and they are filled with 0. The current variable is created, which is an empty list. Different cycles are made to compare which weight is repeated less in each exercise and is stored in the current list. In the end, the amount is added to know how much difference there is with each exercise.

{% highlight ruby %}

  List dpDouble = [];

  for (int i = 0; i < e; i++) {
    dpDouble.add([]);
    for (int j = 0; j < e; j++) {
      dpDouble[i].add(INF);
    }
  }
  for (int row = 0; row < e; row++) {
    dpDouble[row][row] = 2 * dp[row][row];

    for (int revRow = row; revRow >= 0; revRow--) {
      for (int k = revRow; k < row; k++) {
        int minval = 0;
        for (int min = revRow; min < row; min++) {
          if (min == revRow) {
            minval = dpDouble[revRow][min] +
                dpDouble[min + 1][row] -
                2 * dp[revRow][row];
          } else {
            int curr = dpDouble[revRow][min] +
                dpDouble[min + 1][row] -
                2 * dp[revRow][row];
            if (curr < minval) {
              minval = curr;
            }
          }
        }
        dpDouble[revRow][row] = minval;
      }
    }
  }
  return dpDouble[0][e - 1];
}

{% endhighlight %}

In this section, the movements are duplicated, which is the equivalent of removing and putting on each weight, then a for loop is made to find the difference in movements between those that are already common or repeated, and thus generate a combination of exercises. . Finally, it stores in the first list in the last element, the minimum number of movements necessary to be able to do the exercise.
 
<h2>ALTERNATIVE SOLUTIONS</h2> 
 
I find that this procedure is difficult to read and even to imagine, there must be an easier way, especially without using so many nested loops

