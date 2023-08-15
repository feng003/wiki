## mysqldump


    mysqldump -uroot -p'root' -B --all-database > /home/zhang/bak/database_$(date +%F).sql
    mysqldump -uroot -p'root' -B gitea > /home/zhang/bak/gitea_$(date +%F).sql

