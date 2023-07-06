# Getting Started with Github

When you are in a "new" computer, you need to create new keys for that user. You cannot transfer keys from one computer to another, each "computer and user combination" needs its own unique keys.

Become the user and first check if you have an SSH key

    ls ~/.ssh/id_rsa

and if you don't, then generate one (without password)

    ssh-keygen -t rsa -b 4096 -C mikaelhelin@yahoo.com
    ls ~/.ssh/id_rsa

and add your generated key to your public ring

    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub

Copy the key to your clipboard and paste it into your gitHub account. Login into your GitHub account, click on your avatar on the upper-right corner, then choose settings, then choose "SSH and GPG keys", then click on "New SSH key"... name it a title and paste in the key. 
