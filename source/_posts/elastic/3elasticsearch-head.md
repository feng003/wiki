> elasticsearch-head
	

	git clone git://github.com/mobz/elasticsearch-head.git
	
	sudo apt-get install npm
	
	npm config set strict-ssl false
	
	npm install
	
	修改elasticsearch-head下Gruntfile.js文件，默认监听在127.0.0.1下9200端口

    不能放在elasticsearch的 plugins、modules 目录下

    不能使用 elasticsearch-plugin install
    
    1、修改 elasticsearch/config/elasticsearch.yml
    
    2、添加
    
        http.cors.enabled: true
        http.cors.allow-origin: "*"
        //配置信息 key value 之间空格，没有空格报错
    
    3、下载 elasticsearch-head 或者 git clone 到随便一个文件夹
    
    
        
[install](https://segmentfault.com/a/1190000014347757)

[es](https://www.icode9.com/content-3-129394.html)

[error](https://blog.csdn.net/odeng888/article/details/76380832)