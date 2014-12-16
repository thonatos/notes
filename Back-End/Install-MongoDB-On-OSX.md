install MongoDB (OSX).

#### install

	brew install mongodb

#### setting

To have launchd start mongodb at login:

    ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
    
Then to load mongodb now:

    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
    
Or, if you don't want/need launchctl, you can just run:

    mongod --config /usr/local/etc/mongod.conf
    
==> Summary

	/usr/local/Cellar/mongodb/2.6.5: 17 files, 331M