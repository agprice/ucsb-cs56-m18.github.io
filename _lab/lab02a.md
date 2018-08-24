---
layout: lab
num: lab03
ready: false
desc: "Testing and Sorting with Lambdas"
assigned: 2018-08-24 09:30
due: 2018-08-31 12:30
github_org: "ucsb-cs56-m18"
starter_repo: "https://github.com/ucsb-cs56-m18/STARTER_lab03"
---

In this lab:

-   using Maven instead of Ant
-   using packages
-   using lambdas to sort ArrayLists
-   writing your own JUnit tests
-   test coverage


## Working in a pair? Switch navigator/driver frequently and tradeoff who commits

If you are in your repo directory, and type git log at the command
line, you'll see a list of the commits for your repo.

Record that you are pairing on each commit message by putting the
initials of the pair partners at the start of the commit message.

E.g. If Selena Gomez is driving, and Justin Timberlake is
navigating, and you fixed a bug in your `getDanceMoves()` method, your
commit message should be `SG/JT fixed bug in getDanceMoves()`

We should see frequent switches between SG/JT and JT/SG.


Step-by-Step
============

# Step 0: Set up your repo

You may work individually or as a pair on this lab.  However, if you work as a pair, please:
* Work with someone *else* from your same team (i.e. someone else assigned to the same mentor as you)
* Work with someone *different* from who you worked with on lab02.

If there is some reason this is not feasible, please check with your mentor before starting.


Create your repo the same way you did for [lab01](/lab/lab01/) and [lab02](/lab/lab02/)
   * under the <tt>{{page.org}}</tt> organization
   * name should be <tt>{{page.num}}-githubid</tt> OR <tt>{{page.num}}-githubid1-githubid2</tt> as appropriate
   * private, and initially *empty* (no README.md, .gitignore or LICENSE).
   * add your pair partner as a collaborator

Clone this empty repo into your `~/cs56` directory, or wherever you prefer to work.

The starter code is in <{{page.starter_repo}}>.  Visit that page for the approrpiate URL to add the `starter` remote.

To add the starter as a remote, cd into the repo you cloned, then do:

<div>
<tt>git remote add starter {{page.starter_repo}} </tt>
</div>

Then do: 
```
git pull starter master
git push origin master
```

That should get you set up with the starter code.

# Step 1: Get oriented to using Maven instead of Ant

A few things to notice:

* Under `src`, there are two directory trees:
   * `src/main/java/edu/ucsb/cs56/pconrad/menuitems` contains regular Java classes.
   * `src/test/java/edu/ucsb/cs56/pconrad/menuitems` contains the test classes.
   
Don't change the package from `pconrad` to your name; the autograder is looking for the code under the `edu.ucsb.cs56.pconrad.menuitems` package.
So each source file:

* must be under that directory path when it is compiled, and 
* must have `package edu.ucsb.cs56.pconrad.menuitems;` as the first line in the file

Here are the commands you'll need as you work with the code. Try them out now.

| To do this | Type this command |
|-|-|
| compile the code | `mvn compile` |
| reset everything | `mvn clean` |
| run the tests | `mvn test` |
| generate javadoc | `mvn site site:deploy` |
| generate a report of test coverage | `mvn test jacoco:report site:deploy` |
| generate a jar file | `mvn package` |

# Step 2: Set up your github pages site

Try out the `mvn site site:deploy` command and then do a `git add` on the `docs` subdirectory
to get that into github.

Then, go to the Settings page for your repo, and turn on github pages on the `docs` subdirectory of the `master` branch.

Having that page up and working is part of your grade (one of the manually graded items worth, collectively, 20 points).

You can also set up the test case coverage report there.

# Step 3: Start writing code.

In this lab, you'll be implementing several methods of two classes that represent a restaurant Menu, composed of MenuItems.

Here are the methods you'll need to implement:



