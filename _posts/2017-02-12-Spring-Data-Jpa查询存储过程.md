---
layout: post
title:  "Spring-Data-Jpa 查询存储过程"
date:  2017-02-12 21:13:08
categories:  后端
tags:  Oracle Spring-Data-Jpa
excerpt: Jpa使用注解查询存储过程
---

* content
{:toc}

## 简要


*   分享一下今日项目中用到的jpa调用oracle的存储过程。

## 说明

*   存储过程在一般的公司业务中都不推荐使用，原因很简单，不易于维护和管理（因为逻辑在数据库中）

*   最近的《阿里java编码规范文档》中就有提及：

        7. 【强制】禁止使用存储过程，存储过程难以调试和扩展，更没有移植性
 
*   存储过程一般都是在银行业务中使用，把核心逻辑打包成一个存储过程，非常复杂，而在互联网公司并没有多少使用的。

*   此处我使用存储过程原因也是出于项目本身的高并发优化（秒杀），为了减少事务行级锁持有的时间，从而抗住更多的qps，减少业务的反应时间。

## 存储过程

1.  需要先在数据库（Oracle）创建好一个存储过程

    ```sql
    -- 预约存储过程
    CREATE OR REPLACE
    PROCEDURE "STU_YY" (v_username IN VARCHAR2, v_yyccid IN VARCHAR2, v_create_time IN NUMBER,v_pcdm IN VARCHAR2, r_result OUT NUMBER)
    AS
        insert_count NUMBER DEFAULT 0;
        update_count NUMBER DEFAULT 0;
    BEGIN
        -- routine body goes here, e.g.
        -- DBMS_OUTPUT.PUT_LINE('Navicat for Oracle');
    
        UPDATE TCYYB SET YYYRS=YYYRS+1 WHERE id=v_yyccid AND YYYRS<ZDYYRS;
        update_count:=SQL%ROWCOUNT;
    
        IF(update_count=0) THEN
            ROLLBACK;
            r_result:=0;
        ELSIF(update_count<0) THEN
            ROLLBACK;
            r_result:=-1;
        ELSE
            SELECT COUNT(ROWNUM) INTO insert_count FROM TCYYMXB WHERE YYID=v_yyccid AND XH=v_username;
            IF(insert_count>0) THEN
                ROLLBACK;
                r_result:=0;
            END IF;
    
            INSERT INTO TCYYMXB(YYID,XH,CREATE_TIME,PCDM) VALUES(v_yyccid,v_username,v_create_time,v_pcdm);
            insert_count:=SQL%ROWCOUNT;
            IF(insert_count=0) THEN
                ROLLBACK;
                r_result:=0;
            ELSIF(insert_count =1) THEN
                --先查询判断记录是否存在，不存在再插入
                SELECT count(ROWNUM) INTO insert_count FROM TCMDB WHERE XH=v_username AND PCDM=v_pcdm AND MDLY='YY';
                IF(insert_count=0) THEN
                    INSERT INTO TCMDB(XH,PCDM,MDLY,CREATE_TIME) VALUES(v_username,v_pcdm,'YY',v_create_time);
                END IF;
                COMMIT;
                r_result:=1;
            END IF;
        END IF;
        EXCEPTION
        WHEN OTHERS THEN
        ROLLBACK;
        r_result:=0;
    END;
    
    ```

2.  定义实体

    >元数据存储过程可以通过配置 NamedStoredProcedureQuery 在一个实体类型注释。
    
    ``` java
     @Entity
     @NamedStoredProcedureQuery(name = "YY.plus1", procedureName = "stu_yy", parameters = {
             @StoredProcedureParameter(mode = ParameterMode.IN, name = "v_username", type = String.class),
             @StoredProcedureParameter(mode = ParameterMode.IN, name = "v_yyccid", type = String.class),
             @StoredProcedureParameter(mode = ParameterMode.IN, name = "v_create_time", type = Long.class),
             @StoredProcedureParameter(mode = ParameterMode.IN, name = "v_pcdm", type = String.class),
             @StoredProcedureParameter(mode = ParameterMode.OUT, name = "r_result", type = Long.class)})
     public class YY {
         @Id
         @GeneratedValue//
         private Long id;
     }

    ```

3.  创建Repository

    >   存储过程可以从存储库引用以多种方式方法。定义的存储过程被称为可以直接通过 value 或 procedureName 的属性 @Procedure 注释或间接通过 name 属性。 如果没有配置名称的名称用作后备存储库的方法。

    ``` java

    public interface TCYYRepository extends JpaRepository<YY, Long> {
    
        @Procedure(name = "YY.plus1")
        Long stu_yy_procedure(@Param("v_username") String v_username 
                                , @Param("v_yyccid") String v_yyccid
                                , @Param("v_pcdm") String v_pcdm
                                , @Param("v_create_time") Long v_create_time);
   
    }
    ```

4.  调用

    >我们可以根据控制存储过程的输出参数来判断执行的结果，这里就不再列出了，根据实际需求自定义。
    
    ```java

    @Autowired
    private TCYYRepository tcyyRepository;

    Long result = tcyyRepository.stu_yy_procedure(username, yyid, pcdm, timenow);

    ```

5.  资料

    [jpa文档](http://www.zhongtiancai.com/doc/JPA.htm#jpa.stored-procedures)
