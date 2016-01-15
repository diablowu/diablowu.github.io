---
layout: post
title: "oracle导出csv格式文件的function"
permalink: oracle-dump-csv-file
date: 2016-01-15 14:13:34
comments: true
description: "oracle dump csv file"
keywords: ""
categories: tech
tags:
- oracle
---


很久以前需要把oracle的一个表的数据导出为csv格式，于是写了个function，现在不知道有什么更高级的方法了。

<!--more-->

{% highlight sql %}
create or replace function dump_csv( p_query     in varchar2,
                                      p_separator in varchar2,
                                      p_dir       in varchar2,
                                      p_filename  in varchar2)
return number
AUTHID CURRENT_USER
is
    l_output        utl_file.file_type;
    l_theCursor     integer default dbms_sql.open_cursor;
    l_columnValue   varchar2(2000);
    l_status        integer;
    l_colCnt        number default 0;
    l_separator     varchar2(10) default '';
    l_cnt           number default 0;
begin
    l_output := utl_file.fopen( p_dir, p_filename, 'w' );

    dbms_sql.parse(  l_theCursor,  p_query, dbms_sql.native );

    for i in 1 .. 255 loop
        begin
            dbms_sql.define_column( l_theCursor, i,
                                    l_columnValue, 2000 );
            l_colCnt := i;
        exception
            when others then
                if ( sqlcode = -1007 ) then exit;
                else
                    raise;
                end if;
        end;
    end loop;

    dbms_sql.define_column( l_theCursor, 1, l_columnValue,2000 );

    l_status := dbms_sql.execute(l_theCursor);

    loop
        exit when ( dbms_sql.fetch_rows(l_theCursor) &lt;= 0 );
        l_separator := '';
        for i in 1 .. l_colCnt loop
            dbms_sql.column_value( l_theCursor, i, l_columnValue );
            utl_file.put( l_output, l_separator || l_columnValue );
            l_separator := p_separator;
        end loop;
        utl_file.new_line( l_output );
        l_cnt := l_cnt+1;
    end loop;
    dbms_sql.close_cursor(l_theCursor);

    utl_file.fclose( l_output );
    return l_cnt;
end dump_csv;

{% endhighlight %}



    # 授权UTL_FILE
    # dba登录
    # 授权UTL_FILE的使用权限
    grant execute on UTL_FILE to movielib1;

    # 授权创建目录权限（此目录应提前创建）
    grant CREATE ANY DIRECTORY to movielib1;

    # 如果其他用户使用movielib1的function，也应该授权使用
    grant execute on movielib1.dump_csv to star;

    # 创建目录变量 /opt/csv_dump应该提前创建且oracle:dba 组
    CREATE OR REPLACE DIRECTORY DUMP_DIR AS '/opt/csv_dump';

    # 执行导出DUMP_DIR大写
    select movielib2.dump_csv('select * from country_t',chr(9),'DUMP_DIR','country_t.csv') from dual;
