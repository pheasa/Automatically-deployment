# Automatically-deployment
For Developers: Pushing your code from local machine to your server automatically.  Full code here with proper formatting https://gist.github.com/inspiretk/a0a...

Question: what is automatically in this?
Answer: You can push your code from your local machine to your server. So you dont have to upload your code manually, just push it with git.

Cut and paste here for your convenience:
The following steps is for Developers to push their code from their local machine, to their server with git, and let git auto pull the update to your remote folder. How it works: 
- From your local machine, you do your normal coding. When done, you push your new code to git
- Git then updates your local machine, and push it to your server's git
- Git on your server gets the new update, and push it to your server's working folder

log to your remote server as normal:

ssh YourUsername@YourIPorDomain

sudo adduser user sudo

ssh user@server.com

mkdir ~/deploy-folder

git init --bare ~/project.git

cd ~/project.git/hooks

nano post-receive

copy/paste the bash script #!/bin/bash

Copy and paste below script
#!/bin/bash
while read oldrev newrev ref
do
    # only checking out the master (or whatever branch you would like to deploy)
    if [[ $ref =~ .*/master$ ]];
    then
        echo "Master ref received.  Deploying master branch to production..."
        git --work-tree=/home/user/deploy-folder/ --git-dir=/home/user/project.git/ checkout -f
    else
        echo "Ref $ref successfully received.  Doing nothing: only the master branch may be deployed on this server."
    fi
done

# end of file

chmod +x post-receive

type ls [you'll see post-receive file green meaning it's executable now]

type pwd [make sure your file directory is same as your script]

go to your local computer, and go to the folder you want to git. Use git bash program, and go to the folder if you're on windows

cd ~/path/to/working-copy/

git init

git remote add production user@YourServerDomain.com:/home/user/project.git

now make a file, add it to your git, commit it and push it to production server

nano test.txt [then add some text, ctrl x and save it]

git add .

git commit -m 'test1'

git push production master
