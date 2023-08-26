
> 数据库设计
  
  
    用户模块
    商品模块
    订单模块
    仓配模块

### 用户模型设计

    用户实体 姓名 用户名 密码 手机号 email 证件类型 证件号码 性别 邮编 地址 积分 注册时间 生日 状态 用户级别 用户余额
    
    customer_login
    customer_info
    customer_level_info
    customer_login_log
    customer_point_log
    customer_balance_log
    
    用户登录表 登录名 密码 用户状态
    用户地址表 省市区 邮编 地址
    用户信息表 姓名 证件类型、号码 手机号 邮箱 性别
    
    mysql 分区表 hash分区、按范围分区、list分区
    
    show plugins; // partition
    
    在逻辑上为一个表，在物理上存储在多个文件中
    create table `customer_login_log`(
        
    )
    PARITION BY HASH(customer_id)
    PARTITION 4;
    
### 登录日志表

    使用range 分区 按时间 login_time
    以login_time 作为分区键
    
    create table `customer_login_log`(
        
    )
    
    PARITION BY RANGE(YEAR(login_time))(
        PARTITION p0 values les than (2018),
        PARTITION p1 values les than (2019),
        PARTITION p2 values les than (2020),
        PARTITION p3 values les than (2021),
        PARTITION p4 values les than (2022)
    )
    
    //添加分区 分区信息删除以及归档
    alter table customer_login_log add partition ( partition p4 values less than(2018));
    alter table customer_login_log drop partition p0;
    alter table customer_login_log exchange partition p1 with table arch_customer_login_log;

### 商品实体


    product_info 商品信息表
    product_pic_info 商品图片表
    product_category 商品分类表
    product_supplier_info 商品供应商表
    product_comment 商品评论表
    product_brand_info 品牌信息表


### 订单实体


    order_master 订单主表
    order_detail 订单详情表
    order_customer_addr 订单用户地址表
    shipping_info 物流信息表
    order_cart 购物车表
    warehose_info 仓库信息表
    warehose_product 商品库存表
    
### sql 


    SELECT schema_name 
     , SUM(count_star) count  
     , ROUND( (SUM(sum_timer_wait) / SUM(count_star))  
     / 1000000) AS avg_microsec FROM performance_schema.events_statements_summary_by_digest  
     WHERE schema_name IS NOT NULL  
     GROUP BY schema_name; 


    SELECT schema_name , SUM(sum_errors) err_count FROM performance_schema.events_statements_summary_by_digest WHERE schema_name IS NOT NULL GROUP BY schema_name;

    SHOW VARIABLES LIKE 'long_query_time'; 

    SELECT * FROM sys.statements_with_runtimes_in_95th_percentile; 
        
    SELECT * FROM sys.statements_with_errors_or_warnings; 