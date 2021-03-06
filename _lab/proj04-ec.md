---
layout: lab
num: proj04
package: earthquakes
ready: true
desc: "Individual Lab Track Project, Extra Credit"
assigned: 2019-12-01 16:00
due: 2019-12-05 23:59
github_org: "ucsb-cs56-f19"
org: "ucsb-cs56-f19"
gauchospace_url: "https://gauchospace.ucsb.edu/courses/mod/assign/view.php?id=2861299"
---

<div style="display:none" >
Look here for formatted version: http://ucsb-cs56.github.io/f19/lab/proj04-ec
</div>


# This is the EXTRA CREDIT opportunity for the individual lab track

For those on the individual lab track, the three projects (proj01, proj02, proj03) are collectively the project portion of your grade (20% of your final grade).   This project, proj04, if completed, is worth up to 100 points of "extra credit" towards the 1000 points of project points, i.e. 2% extra credit on your final course grade.

You may cooperate with one or more pair partners from your team to help in debugging and understanding the project, but each person should complete the work separately for themselves.

NO Late submissions will be accepted for the extra credit portion.  The extra credit portion is available if and only if ALL of proj01, proj02, proj03 and proj04 are submitted by their respective due dates.  

# Step by step instructions

Continue with the repo and Heroku app you used for the previous steps in the project, i.e. either:

* Github repo: <tt>proj01-githubid</tt>
* Heroku App:  <tt>cs56-f19-proj01-<i>github</i></tt>

OR

* Github repo: <tt>proj02-githubid</tt>
* Heroku App:  <tt>cs56-f19-proj02-<i>github</i></tt>

Either way is fine.  You don't need to create a new repo and Heroku app for proj03.   

# Extra Credit options

To earn the extra credit:

* Complete Step 13 below.
* When finished, do a pull request, and merge that request into master.
* Submit on Gauchospace.

# Step 13: Personal Favorites

In this option, you make it so that each individual user that logs in your application has their own version of the "favorites" (this is more like what would be the case in a real application of this type.)

1. Create a feature branch with an appropriate name.
1. Add a field `uid` to the locations table.  (You do this by adding the field, and it's getters and setters to the appropriate `@Entity` class.  It should have the same data type as the existing `uid` field in the `AppUser` entity.)
2. Whenever you add an item to the locations table, be sure the uid of the currently logged in user is stored in the uid field.  Figuring out where this code goes, and how to write it, is mostly left as an exercise to you.   Just these hints:
   * On any controller method, you can add `OAuth2AuthenticationToken token` as a parameter, and a token with information about the
     currently logged in user becomes available inside that controller method.   Spring Boot takes care of initializing that parameter
     for you.
   * There is example code somewhere in your application already that can compute the `uid` from an `OAuth2AuthenticationToken`; you
     just need to find it and do likewise in the appropriate spot.
3. In a later step, we are going to restrict the listing of favorites to only show favorites for the currently logged in user.
   But, "Admin" users should still be able to see all of the favorites for all users.
   
   So, to make it clear whether the app is working or not, add an `Admin` menu item in the navigation bar that allows you to see
   all of the favorites, regardless of uid.  (In reality, that menu should probably be shown only to Admin users, 
   but we won't implement that in this version.)   
   
   This feature should use a different view; one that has all of the fields from the `locations/index` view, but also shows the `uid` field.   Call this view `locations/admin` (i.e. `src/main/resources/templates/locations/admin.html`).  (Of course, this will need
   a controller endpoint; the code will be similar to the `/locations` controller endpoint.)
   
   Make sure that you can view the locations along with their `uid` values at this new page.  If you have a friend with a GitHub account
   handy, you can ask them to login to your localhost app in a different browser, and try adding some favorites.  Those should show up
   on this page with a differnet uid.  If that isn't possible, don't worry about it for now; we'll test at a later step.
4. Now, to prepare for showing only the individual users favorites, add a `findByUid` method to the `LocationRepository` interface.
   Note that you don't have to write any code for it; you just have to specify the method with the correct naming convention,
   and Spring Boot will make sure that the proper code gets generated.  You need to make sure the return value and parameter types
   and names are correct though.  See other examples of Spring Boot database repositories for examples.
5. Now, in the controller method for the the list of favorite locations at the `/locations` endpoint, use the `findByUid` instead of the `findAll` method, and only show the favorites of the 
   currently logged in user.   This view doesn't need the `uid` field, since it is implied that only favorite locations of the currently
   logged in user are being shown.

   Of course you'll need to know the uid of the currently logged in user so that you can pass it to `findByUid`. You already did that
   once in another controller method, so you'll need that same trick again.
6. At this point, if you've followed all the instructions and  you are able to test, you'll see that things *almost* work,
   but there is still a bug: 
   * When selected, the Admin menu correctly shows all locations for all users
   * When selected, the Favorites menu correctly shows only locations added by the current user
   * BUT, when a user adds a new favorite, they are redirected to a page showing ALL of the favorite locations for all users.
   Locate the root cause of this bug, and fix it.   To do that, you'll need to trace through the code that handles the case of
   adding a new favorite.  Since you wrote that code in an earlier part of the project, you should be able to find it, and know
   how to fix it.
7. Of course, to really know whether any of this is working or not, you'll need to have a separate GitHub user test your app.   If you have a friend with
   a GitHub account handy, you can test on localhost, by opening a different browser.  
   
   But if you can't test with multiple GitHub users on localhost, because no-one else is around, then here's what to do:

   * Do a pull request and accept it to deploy
     your app to Heroku, and then go on the Slack, in `#proj4`  channel and ask for a buddy to help test your app.  Hopefully, they can
     help test yours as well.
    
   * Post a link to your running Heroku app, and indicate you need someone to login,  try add a "favorite location" or two, and then
     ping you back on Slack.

   This should allow you to check whether different users can have different favorites.   You should see the others users show up
   in the users menu, and you should see the other users Favorites show up in the Admin menu, but you should *only* see your
   own favorites under the favorites menu.   And after adding a new favorite, you should see only your own favorites in the page
   to which you are redirected.
   
When all seems to be working, merge into master (if not already done), and proceed to the "final steps" below.
   



# Final Steps

## Final Step 0: A few things to check

1. Look over the staff's working version here:

   * <https://cs56-f19-proj04-v1-pconrad.herokuapp.com/>

   Compare it to your working version on Heroku.
   
   If you see differences, try to determine which of these is true:
   * Is is a minor difference that doesn't matter?
   * Did the staff miss part of the instructions?
   * Did you miss part of the instructions?
   
   If you aren't sure, ask questions on Slack.

## Final Step 1: Check that your code is all on master and Heroku

* Have you pushed all changes to your last feature branch?
* Have you done a final pull request?
* Have you accepted that pull request?
* Have you deployed your master branch to Heroku?
* Do all the parts of your application work? Can you login/logout, and access all pages?

## Final Step 2: Update your javadoc and jacoco report

To update your javadoc and jacoco report, do this:

```
mvn clean
mvn javadoc:javadoc
mvn javadoc:test-javadoc
mvn test
mvn jacoco:report
mvn site
mvn site:deploy
git add docs
git commit -m "xx - update javadoc and jacoco report"
git push origin master
```

## Final Step 3: Check your README.md

Check that your README.md has links to
* your GitHub pages webpage, and that the webpage is published.
* your app running on Heroku
* your repos Travis-CI status

## Final Step 4: Submit on Gauchospace

Then, finally visit <{{page.gauchospace_url}}> and make a submission.

In the text area, enter something like this, substituting your repo name and your Heroku app name:

<div style="font-family:monospace;">
repo name: https://github.com/chrislee123/proj01-githubid<br>
on heroku: https://cs56-{{site.qxx}}-proj01-chrislee123.herokuapp.com<br>
</div>

(Use proj01 or proj02 as appropriate).

Then, **and this is super important**, please make both of those URLs **clickable urls**.

The instructions for doing so are here: <https://ucsb-cs56.github.io/topics/gauchospace_clickable_urls/>


# Grading Rubric:
| item | points | description |
|-|-|-|
|(a)|	20|		README has links to GitHub-Pages (javadoc, etc) and Travis-CI |
|(b)|	20|		In Javadoc, there is apidocs/earthquakes/entities/Location.html and it has getUid indicating Javadoc was updated|
|(c)|	20|		You can login and add a favorite|
|(d)|	20|		There is an admin menu where you can see locations for more than one user|
|(e)|	20	|	On the favorites menu you only see your own favorites |






    
