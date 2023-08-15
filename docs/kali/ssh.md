
    sudo -u git ssh-keygen -t rsa -b 4096 -C "Gitea Host Key"
    sudo -u git cat /home/git/.ssh/id_rsa.pub | sudo -u git tee -a /home/git/.ssh/authorized_keys
    udo -u git chmod 600 /home/git/.ssh/authorized_keys


    sudo -u zhang  ssh-keygen -t rsa -b 4096 -C "GitHub Host Key"
    cat id_rsa.pub
    ssh -T git@github.com