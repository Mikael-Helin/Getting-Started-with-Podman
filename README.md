# Gettings Started with Podman

Follow these 3 steps to go. 1) Github 2) Podman 3) VS Code

## Github

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

Copy the key to your clipboard and paste it into your gitHub account.
* Login into your GitHub account,
* click on your avatar on the upper-right corner,
* then choose settings,
* then choose "SSH and GPG keys",
* then click on "New SSH key"... name it a title and paste in the key... and submit

## Podman

Install synaptic and from synaptic install podman, skopeo and buildah.

Let us create a Website. First take the Debian Bullseye as the base image to build a new image, my-first-image

    mkdir -p Projects/MyFirstPod
    cd Projects/MyFirstPod
    cat << 'EOF' > Dockerfile
    FROM debian:bullseye
    COPY installer.sh /
    RUN chmod +x /installer.sh
    RUN /installer.sh
    COPY runner.sh /
    RUN chmod +x /runner.sh
    CMD ["COPY installer.sh /
    EOF
    cat << 'EOF' > installer.sh
    apt-get update
    apt-get install -y nginx
    EOF
    cat << 'EOF' > runner.sh
    touch /var/www/.lock

    cleanup() {
      /etc/init.d/nginx stop
      /usr/bin/rm /var/html/.lock
    }

    trap 'cleanup' SIGTERM
    
    while true; do
      /etc/init.d/nginx start
      if [ ! -f "/var/www/.lock" ]; then
        break
      fi
      sleep 10
    done

    cleanup
    EOF
    podman build -t my-first-image .

then let's create a folder for html and start a container.

    mkdir html
    echo "Hello World!" > html/index.html
    podman run --name my-first-container -p 8080:80 -v ./html:/var/www/html my-first-image
    curl localhost:8080

And if you want to stop the container, you just type

    podman stop my-first-container

and to start it again

    podman start my-first-container

## VS Code

The last part here is to use VS Code with the project

    cd ~/Projects/MyFirstPod/html
    code .

You are ready to go!
