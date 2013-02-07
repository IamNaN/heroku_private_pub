## Heroku + private_pub ##

### Background ###

Faye ([github](http://github.com/faye/faye)) is a rack-based messaging system that allows web clients to communicate. 

Ryan Bates created private_pub ([github](http://github.com/ryanb/private_pub)) to easily add Faye to Rails projects. There is a Railscast for that [here](http://railscasts.com/episodes/316-private-pub).

Heroku is a common place for us rails developers to put our projects, but Heroku can be restrictive given memory and processor constraints, especially at the free level. Running any Faye server concurrently with a rails app isn't stable.

This project is a barebones private_pub deployment that runs as a separate app on Heroku. It makes it easy to get your own Faye server running in just a few minutes.

### Installation ###

Setting up heroku_private_pub is easy: just clone it, edit one file, and push it to Heroku. Of course, there's a little handwaving going on, so here are the excruciating details.

In this example, we want to create a Faye server named 'mypubserver' that will  run at http://mypubserver.herokuapp.com/faye. So, anywhere you see 'mypubserver', replace it with the name you want to use.

1. Clone the heroku_private_pub on github to your development computer, in a new project folder (not the same as your rails project):

   ```
   ~> cd projects
   ~/projects> git clone git://github.com/IamNaN/heroku_private_pub mypubserver
   Cloning into 'mypubserver'...
   remote: Counting objects: 15, done.
   remote: Compressing objects: 100% (11/11), done.
   remote: Total 15 (delta 3), reused 14 (delta 2)
   Receiving objects: 100% (15/15), done.
   Resolving deltas: 100% (3/3), done.
   ~/projects> cd mypubserver
   ~/projects/mypubserver> 
   ```
2. Open `config/private_pub.yml` in your text editor and replace MY_KEY with what you have in the `private_pub.yml` of your rails project. Also, replace MY_APP with the name of the heroku app, mypubsever in this case (this is different from the name of your rails app). Save this file.
3. Run the bundler to install the gems
   ```
   ~/projects/mypubserver> bundle install
   ```
3. Commit the changes locally.
   ```
   ~/projects/mypubserver> git commit -am "Adding my configuration info and bundle"
   ```
5. Install the heroku gem if you haven't already.
   ```
   ~/projects/mypubserver> gem install heroku
   ```
   Note: The folks at Heroku are "sunsetting" (a nice way of saying leaving you in the dark) the heroku gem soon in favor of a less railsy thingy called toolbelt. I'll be sunsetting Heroku when they sunset the heroku gem, so maybe someone else can contribute that update to this document.
6. Create your app on Heroku with the heroku tool.
   ```
   ~/projects/mypubserver> heroku apps:create mypubserver
   Creating mypubserver... done, stack is cedar
   http://mypubserver.herokuapp.com/ | git@heroku.com:mypubserver.git
   Git remote heroku added
   ```
7. Push it all up to heroku.
   ```
   ~/projects/mypubserver> git push heroku master
   ```

It should start right away. After you have verified everything is working as expected (see *Is It Running* below) you can delete this project. However, it's handy if you ever want to peek at the traffic going through your Faye server (see *What's It Saying?* below).

If it is unlikely you'll ever need it again, then go ahead and delete the project folder. But! if you find yourself needing it again, you can just pull it from Heroku thusly:
```
git clone git@heroku.com:mypubserver.git mypubserver
```

Make your changes, commit them, and push them again just like we did above.

### Troubleshooting ###

#### Is It Running? ####
You can test that your new Faye server running properly by browsing to the app. In the above example, we would go to http://mypubserver.herokuapp.com should see a plain text message "Sure you're not looking for /faye?"

#### Is It Configured? ####
If that is working but your messages aren't finding their way between your app and clients, double check the settings in `config\private_pub.yml` match those in the same file in your rails project.

#### What's It Saying? ####
If that's correct but you're still seeing problems, then check out the log to see what the problem is.
```
~/projects/mypubserver> heroku logs --tail
```
