---
title: "5_1SQL语句"
date: 2020-03-13
tag: "create source alter" 
---

> create


    CREATE DATABASE [IF NOT EXISTS] <数据库名>
    [[DEFAULT] CHARACTER SET <字符集名>] 
    [[DEFAULT] COLLATE <校对规则名>];

    create database jike_sql default character set utf8 default collate utf8_general_ci;

    MariaDB [jike_sql]> explain select * from heros_without_index where name='刘禅';
    +------+-------------+---------------------+------+---------------+------+---------+------+------+-------------+
    | id   | select_type | table               | type | possible_keys | key  | key_len | ref  | rows | Extra       |
    +------+-------------+---------------------+------+---------------+------+---------+------+------+-------------+
    |    1 | SIMPLE      | heros_without_index | ALL  | NULL          | NULL | NULL    | NULL |   69 | Using where |
    +------+-------------+---------------------+------+---------------+------+---------+------+------+-------------+

    alter table heros_without_index add index name(`name`);

    MariaDB [jike_sql]> explain select * from heros_without_index where name='刘禅';
    +------+-------------+---------------------+------+---------------+------+---------+-------+------+-----------------------+
    | id   | select_type | table               | type | possible_keys | key  | key_len | ref   | rows | Extra                 |
    +------+-------------+---------------------+------+---------------+------+---------+-------+------+-----------------------+
    |    1 | SIMPLE      | heros_without_index | ref  | name          | name | 767     | const |    1 | Using index condition |
    +------+-------------+---------------------+------+---------------+------+---------+-------+------+-----------------------+

    DROP TABLE IF EXISTS `user_gender`;
    CREATE TABLE `user_gender`  (
      `user_id` int(11) NOT NULL,
      `user_name` varchar(255) CHARACTER SET utf8mb4  NOT NULL,
      `user_gender` tinyint(1) NOT NULL,
      PRIMARY KEY (`user_id`) USING BTREE
    ) ENGINE = InnoDB CHARACTER SET = utf8mb4 ROW_FORMAT = Dynamic;
    source /home/centos/user_gender.sql