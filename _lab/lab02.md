---
layout: lab
num: lab02
ready: true
desc: "Polynomials"
assigned: 2018-08-16 09:30
due: 2018-08-27 23:59
github_org: "ucsb-cs56-m18"
starter_repo: "https://github.com/ucsb-cs56-m18/STARTER_lab02"
---

In this lab:

-   using inheritance
-   using an `ArrayList`
-   overriding `equals`
-   overriding `toString`
-   problem solving

Step-by-Step
============

# Step 0: Set up your repo

This lab will build on the material we introduced about Ant and JUnit in [lab01](/lab/lab01/).

You will also apply what you are learning from your reading about inheritance, object equality, and using the ArrayList class.

You may work as a pair, or individually on this lab (your choice).  Name your repo as appropriate.

Create your repo the same way you did for [lab01](/lab/lab01/), this time make sure to initialize the repo with a .gitignore and README.md:

   * under the <tt>{{page.org}}</tt> organization
   * name should be <tt>{{page.num}}-githubid</tt> OR <tt>{{page.num}}-githubid1-githubid2</tt> as appropriate
   * private, and initially empty.
   * add your pair partner as a collaborator

Then, create an empty directory for lab02, do a `git init` and then add remotes just as you did for lab01.

The starter code is in <{{page.starter_repo}}>.  Visit that page for the approrpiate URL to add the `starter` remote.

## If pairing, register your pair on Gradescope

On Gradescope, on the page for {{page.num}}, there should be an option to join a group.  Please use this to add your pair partner for this assignment.

## Working in a pair? Switch navigator/driver frequently and tradeoff who commits

If you are in your repo directory, and type git log at the command
line, you'll see a list of the commits for your repo.

Record that you are pairing on each commit message by putting the
initials of the pair partners at the start of the commit message.

E.g. If Selena Gomez is driving, and Justin Timberlake is
navigating, and you fixed a bug in your `getDanceMoves()` method, your
commit message should be `SG/JT fixed bug in getDanceMoves()`

We should see frequent switches between SG/JT and JT/SG.


## Set up your javadoc

Set up github pages to publish your javadoc just as we did for lab01.

# Step 1: A few small adjustments to the build.xml


(1) Make one small change to your `build.xml` if it is not already there. (It may already be there, in which case there is nothing to DO, but you should still read the EXPLANATION here to understand what these two lines of `build.xml` code do.)

-   Find the <javac ...> task inside the `compile` target.
-   Add these attributes.

```xml
  debug="true" debuglevel="lines,vars,source"
```

For example, before you might have:

```xml
   <javac srcdir="src" destdir="build" includeantruntime="false"  >   
```
Change this to:

```xml
    <javac srcdir="src" destdir="build" includeantruntime="false" debug="true" debuglevel="lines,vars,source" >   
```

Note that this is called "adding two attributes to an open tag". This
is important to know for the exam (i.e. what a tag is, and what an
attribute is.)

Adding these attributes will help improve the error messages you get
when a program throws an exception. For example, instead of:

```
    -bash-4.2$ java Foo
    Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 10
        at Foo.main(Unknown Source)
    -bash-4.2$ 
```

You'll get the following:

```
    Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 10
        at Foo.main(Foo.java:7)
```

Note that the output now includes a way of
finding what line of source code was executing when the exception
occurred: the name of the file, and the line number of source code in
that file. Note how:

-   above, the error message says that the error in Foo.main was at: `(Unknown` `Source)`
-   below, the error message says that the error in Foo.main was at: `(Foo.java:7)`

(2) Change the run target of your build.xml to run the main
from the `Polynomial` class. Test this. You'll probably get a "usage"
message indicating that there aren't enough command line
arguments. That's not a problem—you'll actually be running this from
the command line most of the time (more on that later.) Just make sure
that the ant `run` target actually does depend on the `compile`
target, and that the code compiles and runs.

(3) Change the jar task so that the name of the jar file created is `polynomial.jar` instead of `rational.jar`.

Also, change the name of the main that it loads into the manifest for the jar file.

(4) Commit all these changes:

-   `git status`
-   `git add ...` (list all the files you changed or added)
-   `git status`
-   `git commit -m "XX/YY what you did"`
-   `git push origin master`

Step 2: A note about the .git directory and the .gitignore file
---------------------------------------------------------------

If you aren't already there, cd into your repo directory.

-   repo is short for "repository"; your repo directory is something like ~/cs56/{{page.num}}-github1-github2
-   from now on, I'll just say "your repo directory" and assume you know what I mean.

As you may know, in Unix, "hidden files" are those that start with a dot.

-   There is one hidden directory in your repo, called .git
-   There is one hidden file in your repo called .gitignore

The `-a` flag of the Unix `ls` command can be used to list hidden files in directory listings.

-   You can use the `ls` `-a` (pronounced: ell ess minus ay) to get a short form directory list that includes these files.
-   You can use the `ls` `-al` (pronounced: ell ess minus ay ell) to get a long form directory list that includes these files.

### What is the .git directory?

NOTE: The contents of this section may be something you are tested on on Midterm Exam 1 and subsequent exams, so READ CAREFULLY!

The .git directory is the magic ingredient that turns an ordinary directory into a git repository.

-   It ONLY exists in the root directory of the repository.
-   It's presence causes EVERYTHING under that directory (all subdirectories, and sub-sub directories, etc) to be considered part of the repository
-   It's presence or absence is the ONLY difference between a plain old directory and a git repository.
-   That directory contains the ENTIRE HISTORY of the repository--every commit, every commit message, every version of every file, EVERYTHING.
-   It is stored in an intelligent data structure that uses lots of compression and algorithmic tricks to make it both small and fast.
-   Normally, you DO NOT NEED to mess with stuff in this directory, except sometimes making changes in the .git/config file.
-   Normally, you SHOULD NOT mess with stuff in this directory directly unless you REALLY know what you are doing.
    -   Instead, we use "git" commands that manipulate the contents of this directory while preserving its structure.

For now, that's all you need to know about the contents of the .git directory. However, if you want to learn more, you are encouraged to read ahead in the GIT pocket guide. The internals of git are fascinating, and the more you know, the more power you will have to make git do all kinds of wonderful tricks.

### What is the .gitignore file?

The .gitignore file is NOT inside the .git directory, but instead is considered a regular file in the repository.

-   That is, if you change it, you need to "git add", "git commit" and "git push" to make the changes take effect, just like any regular file in your repo.

BUT, the .gitignore file has special powers over the way that git operates.

Generally speaking, when we set up a repository, we want to store only SOURCE CODE:

-   For Java, we store the .java files in the repo, but NOT the .class or .jar files
-   For C/C++ we store the .c, .h, .cpp files in the repo, but NOT the .o files, and executables
-   etc.

We also don't want to store backup files, such as the files that emacs creates that end in tilde, such as Foo.java~, Bar.java~, etc.

The .gitignore helps us do that. Use the more command to list the contents of your .gitignore file, which will look something like this (if you created a .gitignore for Java and it has more contents than this, that is fine):

    -bash-4.2$ more .gitignore
    *~
    *.class
    -bash-4.2$

You see that there are two lines in this file.

-   The first line uses a wildcard pattern (\* means wildcard) to say that "any file that ends in ~" should be ignored by git.
-   The second line says: "any file that ends in .class should be ignored by git".

You will see that when you do "git status" in this repo, the backup and .class files don't show up. They are ignore by git. And that's the way we want it.

Note: Remember how we're using a specific version of JUnit for testing? If you want that specific .jar file to be uploaded with your repository, you'll need to add the following line when you're `git add`-ing files for the first time:

    git add --force lib/junit-4.8.2.jar
    
The --force flag is FORCING git to track the file despite the .gitignore file not tracking any `.jar` files.

### What YOU NEED TO DO for this step

You need to add one more lines into the .gitignore

```
.ant-targets-build.xml
```

Optionally, if you use emacs, you may want to add this:

```
*~
```

The `.ant-targets-build.xml` line says to ignore any file with this name. That file is one that is used by ant to cache some intermediate results when you do certain ant commands. We don't want to store that in the repo.

Make those changes, then commit them:

-   git add .gitignore
-   git commit -m "improved .gitignore"
-   git push origin master

### Summary of this Step

In short:

-   Leave the `.git` directory alone unless you know what you are doing
-   Modify the `.gitignore` anytime there is some other kind of file that you also want git to ignore




Step 3: Java Programming
------------------------

Your programming task in this lab can be described very simply:
replace all the stubs in Polynomial.java with code that

-   implements the methods described in the javadoc comments
-   passes the unit tests in PolynomialTest.java
-   passes any other reasonable unit tests that the TAs might devise!

It is possible (though unlikely) that there may be typos/errors in the
unit tests themselves. If you suspect this, please post something on
the Gauchospace forum "Report Errors in Unit Tests Here". The FIRST
person/pair partner to CORRECTLY identify each incorrect unit test
(supplying a correct test in its place) will receive extra credit.

If there are corrections to the tests, that information will be sent
out to you by email and posted on the "Instructor Announcements" forum
in Gauchospace.

Do NOT simply change the `PolynomialTest.java` file. We will test your
program against the original `PolynomialTest.java` file (with any
corrections that are distributed).

You may ADD tests to `PolynomialTest.java`, but you should not CHANGE the
tests that are listed there or delete tests, unless you have been told
to do so in an email from the instructor.

For any other typo corrections, please post to Piazza.

I suggest that you work in this order. There are also three hints for
debugging that follow this suggested order of work. You may want to
read though ALL of the instructions in Step 2 before starting
anything.

### Suggested order of work

-   (1) Generate the javadoc (`ant javadoc`) and look through the descriptions of all the methods. For each method, look through PolynomialTest.java and find test cases that use that method. Try to understand what each method is supposed to do.

-   (2) The first methods I suggest you undertake are `degreeCoeffsHighToLow`, `degreeCoeffsLowToHigh`, `lowToHigh`, and `highToLow`. These are the easiest to understand, and the easiest to get their tests to pass. Plus, you may find these methods handy in implementing other methods later.

-   (3) Next, I HIGHLY RECOMMEND that you get the constructor from the int \[\] and the toString methods working before doing anything else. Having these correct will help you with the output of the JUnit test cases for the other methods. If toString isn't correct, the JUnit output won't make any sense.

-   (4) Next, I HIGHLY RECOMMEND that you get `.equals` working. Keep in mind that JUnit uses the `.equals` method internally to when it compares one Polynomial to another. If `.equals()` doesn't work correctly for your class, then JUnit isn't going to work correctly.

-   (5) Next, tackle the constructors.

-   (6) Finally, do `plu`s THEN `times`, and save `minus` for last. If you do it in that order, you'll find that you can implement `minus` with a clever time-saving trick. Hint: It does NOT involve copy and paste.

<div style="text-align:center; font-weight:bold;">
Helper functions are a good idea
</div>

If you find that a certain method is getting a bit too complex, or if you find that you are repeating yourself a lot, you may want to factor some code out into helper methods.

There are two approaches to dealing with helper methods, each of which can be argued:

-   Make them private, since they are helper methods and no code outside the class should be using them.
-   Make them public, so that they can be easily unit tested.

My suggestion is: make them private UNLESS you write unit tests. And writing unit tests is a VERY GOOD IDEA. But, that's up to you.

### Additional debugging help

There are three additional things that can help while doing your debugging:

-   (1) There is a main program that you can run to test basic Polynomial operations and printing of Polynomials. Here are some examples of the Unix command line to run that program. Note that you need single quotes around the Polynomials, and that the \* operator needs a backslash in front of it (so the Unix shell doesn't turn into a "wildcard" for filenames.) You can use this main to see what happens when you take in a Polynomial and then print it back out with the toString() method. If you pass just ONE polynomial in the command line, it simply gets "constructed" from the string, and then printed back out with toString. If you are using "debug print statements" (see item 2 below) then it can be hard to make sense of those in the JUnit test output. But, running the main, you can isolate what's happening with just ONE polynomial at a time, and see your debugging print statements in a less cluttered, sensible order.
    -   `java -cp build Polynomial '10x^2 - 20x + 3'`
    -   `java -cp build Polynomial '2x + 3' + '4x - 5'`
    -   `java -cp build Polynomial '2x + 3' \\\* '4x - 5'`
    -   `java -cp build Polynomial '2x + 3' - '4x - 5'`


-   (2) There is a public static boolean variable called `debug` inside the Polynomial class. You can set this to true, and then print debugging output. You'll see examples of that in the constructor that creates a Polynomial object from a String.

By the way, the constructor that creates a Polynomial object from a String is already written for you, and is, to the best of our knowledge, correctly implemented. It should pass its tests once the other methods are correctly implemented—however, until you get toString, the constructor that takes an int \[\], and the .equals method working, however, some of its tests may fail.)

When all the tests pass, you may then add additional tests for other kinds of polynomials (both well formed and badly formed) that are not covered by the tests that you see in PolynomialTest.java. The number of such tests is up to you. There is such as thing as too many tests, and there is such a thing as not enough tests. Finding the right balance is part of the exercise here.

The reason for adding the additional tests is to try to anticipate what the "secret" tests might be testing for, i.e cases not already covered by the tests in PolynomialTest.java.

When you feel you have tested sufficiently, and all your tests pass, you are ready for your final check over, then submission on Gauchospace.

-   (3) If you need to, for debugging purposes, you may add an additional class called PolynomialDebugging. That class might look something like this:

```java

    public class PolynomialDebugging {

     public static void main (String [] args) {
         if (args[0]=="1") {
            // Put some code you want to test here
         } else if (args[0]=="2") {
            // Put some code you want to test here
         } else 
        .. // continue in this fashion
     }

    }
```

The point of this is to be able to isolate one small thing that you
are trying to debug, independent of all the confusing output of all
the many test cases, and so you can see your debugging print
statements more easily. If you do this, please DO git add, git commit,
git push the PolynomialDebugging class. Note that we won't grade that
class---and you aren't required to use one. This is just a suggestion
in case you find it helpful.

-   (4) Another helpful thing to do might be this: introduce a separate ant target that calls the junit task test, but only on one specific test at a time, rather than all of them. You might call it `<target name="onetest" depends="compile">`. You can start by copying/pasting the inside of the test target, but then you'll have to look up the junit ant task and see how to specify which test gets run.

Step 3: Preparing to submit: final code walkthrough against rubric
------------------------------------------------------------------

Now, you are almost done. The last four steps are to:

-   check your code to be sure you will maximize your grade, and
-   generate the final version of your javadoc
-   do a final commit and push for any last minute changes
-   submit on Gauchospace.


### Updating javadoc

If you are working along, right before you do the final submission, do a "git commit", "git push" and then run "ant javadoc".

Then do the steps to publish the updated javadoc to your public repo.


### Final git pull, git status, git add, git commit, git push origin master

Go through the git steps one last time in case you made any last minute changes. Be sure that your repo is up to date. When it is, you are ready to submit on Gauchospace.


Step 5: Submitting via Gauchospace
--------------------------------


Please download a `.zip` file from your github repo and submit it to the {{page.num}} entry on Gradescope.

* If you worked in a pair, please register your pair as a *group* on Gradescope for {{page.num}} before submitting.  One partner's submission counts for both of you.


# Grading:

* Part of your grade is determined by the points shown on Gradescope
* The remainder is determined by the following:
   * Did you follow the instructions for setting up your repo? (naming, making it private, pulling in starter code,
     adding partner as collaborator)
   * Did you publish your javadoc correctly, link to it from your README, and in general, tidy up your README, and make changes to build.xml as indicated?
