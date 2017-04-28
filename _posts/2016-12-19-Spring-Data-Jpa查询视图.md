---
layout: post
title:  "Spring-Data-Jpa 查询视图"
date:  2016-12-19 20:00:50
categories:  后端
tags:  Oracle Spring-Data-Jpa
excerpt: Oracle Spring-Data-Jpa
---

* content
{:toc}

## 简要

*   今天在做项目时需要用到使用 Spring-Data-Jpa查询Oracle数据库视图的功能，google了好多得到最后的经验分享给大家

## 说明

*   视图虽然在处理上和普通的 Table 没有多大的区别，但还是有许多需要注意的细节：
    
    1.视图对应的实体类不需要任何注解，里面只有视图对应的 Column 以及构造器和Get/Set方法就可以了；
    
    2.不能创建Repository接口，总会报错找不到表或者视图，这个我也不知道为什么(可能是因为视图没有id的原因)；
    
    3.基本上是使用原生的SQL语句执行，封装的Spring-Data-Jpa应该是不能使用，查询出数据自己封装；
    
## 实例

1.  需要先在数据库（Oracle）创建好视图，执行一遍sql语句保证能查询到信息

    `select * from  NEWJW.XS_XJB_VIEW t`

2.  创建基础的EntityManage

    ``` java
     import javax.persistence.EntityManager;
     
     /**
      * Created by jiaohongwei on 2016/12/19.
      */
     
     public interface ObjectDaoService {
         EntityManager getEntityManager();
     }
    
    ```
    
    ``` java
    import org.springframework.stereotype.Service;
    
    import javax.persistence.EntityManager;
    import javax.persistence.PersistenceContext;
    
    /**
     * Created by jiaohongwei on 2016/12/19.
     */
    @Service
    public class ObjectDaoServiceImpl implements ObjectDaoService {
       
        @PersistenceContext
        private EntityManager entityManager;
    
        @Override
        public EntityManager getEntityManager() {
            return this.entityManager;
        }
    }
    
    ```

3.  创建实体类

    ``` java
    
    // domain 中不需要任何的注解，这是一个映射到视图的enity,并没有真实的表。
    public class KdOrView implements Serializable {
    
    	/**
    	 * 
    	 */
    	private static final long serialVersionUID = 1L;
    	// KD Table Field
    	private String id;
    	private String user_name;
    
    	// Or Table Field
    	private String userid;
    
    	public KdOrView() {
    
    	}
    
    	public KdOrView(String id, String user_name, String userid) {
    		this.id = id;
    		this.user_name = user_name;
    		this.userid = userid;
    	}
    
    	// GET SET....... 省略
    
    }

    ```

4.  开始查询

    ```java
    
    @Service
    public class UserCertificationServiceImpl implements UserCertificationService {
    	
    	@Resource
    	private ObjectDaoService objectDaoService;
    
    	public List<KdOrView> findKdByNativeSQL() {
    		// 1.组织sql语句，这里设置参数 ":id" 方式，还有一种是 "?1,?2"，需要指定
    		String sql = "select id,user_name,userid from NEWJW.XS_XJB_VIEW k where k.id = :id";
    		// 2.创建实体管理对象
    		EntityManager entityManager = objectDaoService.getEntityManager();
    		// 3.使用jpa 包装查询，获取query 对象
    		Query query = entityManager.createNativeQuery(sql);
    		// 4.再使用去包装查询获取 nativeQuery 对象
    		SQLQuery nativeQuery = query.unwrap(SQLQuery.class);
    		// 5.设置参数，对应上面的参数 ":id"
    		nativeQuery.setParameter("id", "1");
    		// 设置返回值类型Map，然后对Map进行处理。
    		nativeQuery.setResultTransformer(CriteriaSpecification.ALIAS_TO_ENTITY_MAP);
    		// 6.执行查询，获取viewlist
    		List<KdOrView> viewlist = new ArrayList<KdOrView>();
    
    		// 解析viewlist，根据功能逻辑封装
    		@SuppressWarnings("unchecked")
    		List<Map<String, Object>> retVal = nativeQuery.list();
    		if (retVal != null && retVal.size() > 0) {
    			for (Map<String, Object> map : retVal) {
    				KdOrView kv = new KdOrView();
    				// key字段全部要大写，这里使用的是oracle数据库
    				kv.setId(map.get("ID").toString());
    				kv.setUser_name(map.get("USER_NAME").toString());
    				kv.setUserid(map.get("USERID").toString());
    				viewlist.add(kv);
    			}
    		}
    
    		return viewlist;
    	}
	}

    ```
