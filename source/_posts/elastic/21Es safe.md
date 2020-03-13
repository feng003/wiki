> safe 


    身份认证

        用户名和密码
        密钥或kerberos票据
        Realmx

    用户鉴权 RBAC

        User
        Role
        
        Permission
        Privilege
            Cluster Privileges
                all/monitor/manger/manger_index/manage_index_template/manage_rollup
            Indices Privileges
                all/create/create_index/delete/delete_index/index/manage/read/write view_index_metadata



user | role
---|---
elastic | Supper User
kibana  | kibana connect and communicate
logstash_system | logstash 
beats_system    | beats
apm_system      | APM
Remote_monitoring_user | Metricbeat 


    POST /_security/user/admins 
    {
      "password": "admins",
      "roles": ["admin","roles"],
      "full_name": "admins ss",
      "email":"1@qq.com",
      "metadata": {
        "intelligence":7
      }
    }
    
    x.pack.security.enabled = true
    bin/elasticsearch-password interactive
    
    bin/elasticsearch -E node.name=node0 -E cluster.name=ys -E path.data=node0_data -E http.port=9200 -E xpack.security.enabled=true
    

    传输加密

    日志审计