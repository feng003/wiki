## git

    git push error GnuTLS recv error (-110): TLS 链接非正常地终止了

    git config --global http.sslVerify false
    git config --global http.postBuffer 1048576000


## gitea 

    export GITEA_WORK_DIR=/var/lib/gitea/
    
    wget -O gitea https://dl.gitea.com/gitea/1.20.2/gitea-1.20.2-linux-amd64

    chmod +x gitea

    sudo mkdir -p /var/lib/gitea/{custom,data,log}
    sudo chown -R git:git /var/lib/gitea/
    sudo chmod -R 750 /var/lib/gitea
    sudo mkdir /etc/gitea
    sudo chown root:git /etc/gitea
    sudo chmod 770 /etc/gitea

    sudo adduser --system --shell /bin/bash  --gecos 'Git Version Control'  --group   --disabled-password   --home /home/git  git

    cd /etc/systemd/system
    vim gitea.service

    sudo systemctl enable gitea
    sudo systemctl start gitea
    sudo systemctl status gitea

