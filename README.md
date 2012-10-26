## Heroku + private_pub ##

### Background ###

Faye ([github](http://github.com/faye/faye)) is a rack-based messaging system that allows web clients to communicate. 

Ryan Bates created private_pub ([github](http://github.com/ryanb/private_pub)) to easily add Faye to Rails projects. There is a Railscast for that [here](http://http://railscasts.com/episodes/316-private-pub).

Heroku can be restrictive given memory and processor constraints, especially at the free level, so running any Faye server concurrently with a rails app isn't stable.

This project makes it easy by giving you a barebones private_pub that runs as a separate app on Heroku.

### Installation ###

Setting up heroku_private_pub is pretty easy, just clone, edit one file, and push to Heroku. Of course, there's a little handwaving for you, so here are the excruciating details.

1. Clone heroku_private_pub to your development computer, in a new project folder (not the same as your rails project). In this example we'll call it 'mypubserver':

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
2. Open `config/private_pub.yaml` and replace MY_KEY with the same one you have in the `private_pub.yml` of your rails project. Also, replace MY_APP with the name of the heroku app where *this* private_pub server will be install (this is different from the name of your rails app). Save this file.
3. Run the bundler to install the gems
   ```
   ~/projects/mypubserver> bundle install
   ```
3. Commit the changes locally.
   ```
   ~/projects/mypubserver> git commit -am "Adding my configuration info"
   ```
5. Install the heroku gem if you haven't already. They are "sunsetting" (a nice way of saying leaving you in the dark) the heroku gem soon in favor of a less-rails-y toolbelt. I'll abandon heroku when they abandon the gem, so maybe someone else will contribute that update.
   ```
   ~/projects/mypubserver> gem install heroku
   ```
6. Create your heroku app with the heroku tool installed by the gem.
   ```
   ~/projects/mypubserver> heroku apps:create mypubserver
   Creating mypubserver... done, stack is cedar
   http://mypubserver.herokuapp.com/ | git@heroku.com:mypubserver.git
   Git remote heroku added
   ```
7. Push it to heroku.
   ```
   ~/projects/mypubserver> git push heroku master
   ```

After you have verified everything is working as expected (see Troubleshooting below) you can delete this project. However, it is handy if you ever want to watch the traffic going through your Faye server (see "What's It Saying?" below).

If it is unlikely you'll ever need it again. In the even that you find yourself needing it again, you can just pull it from Heroku thusly:
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
