## 1、每写完一个sql，用`explain`执行一下，避免全部扫描（建议）

- `explain` 中``type`` 连接类型
 
 |  类型名称（执行由快到慢）   | 说明  | 示例  |
 |  ----  | ----  | ----  |
 | `const`  | 命中主键、唯一(unique)索引或者是一个常量值 | `explain select t.* from mds_user t where t.id = 1316209505075466241;`|
 | `eq_ref`  | 连表查询，命中主键 | `explain select * from mds_user t join mds_dept_user d on d.user_id = t.id;` |
 | `ref`  | 命中非主键/非唯一的索引等值扫描 |   ` CREATE INDEX mds_user_status_IDX USING BTREE ON mds_user (status);` ` explain select t.id from mds_user t where t.status = 1;`|
 | `range`  | 命中范围扫描，部分索引扫描 | `explain select t.* from mds_user t where t.id > 1316209505075466241`|
 | `index`  | 命中索引树扫描 | `explain select t.id from mds_user t `|
 | `all`  | 全表扫描 | `explain select t.* from mds_user t `|
 
 ## 2、`delete`或`update`操作时，后面追加`limit`（强制）
```sql
delete from mds_user t where t.status = 2 limit 10;
```
- 降低写错SQL的代价
> 如果没加`where`条件，至少只被删除了10条
- sql效率很可能更高
> 只会命中符合条件的前10条，若是没有`limit`，还会继续扫描
- 容易把CPU拉满
> 若删除的数据量大时，容易把CPU拉满，导致越删越慢

## 3、设计表时，表、字段都有相应的注释（强制）
描述简洁，易理解，枚举类型必须说明所有类型，禁止描述业务采用疑问词修饰以及估计后期会用到的扩展字段（后期用到再说），永远要记住注释是给别人看的！！！
```sql
create table `sys_user_info` (
  `id` bigint(20) not null comment '用户主键',
  `status` tinyint(4) NOT NULL COMMENT '状态 1/启用 2/停用',
  `gender` tinyint(4) NOT NULL COMMENT '性别 1/男 2/女 是否要考虑后期可能存在未知和未说明的情况？存疑', -- 典型反例
  primary key (`id`) using btree
) engine=innodb default charset=utf8 row_format=dynamic comment='用户信息表'
```
- 良好的可读性
>
## 4、设计表基本字段：`主键`、`创建时间`、`更新时间`、`创建人`、`更新人`、`状态`，中间表除外（建议）
优点：关联、查询、排序、稽查和审计、数据增量同步、状态控制
```sql
create table `sys_user_info` (
  `id` bigint(20) not null comment '用户主键',
  `created_time` datetime not null comment '创建时间',
  `updated_time` datetime not null comment '结束时间',
  `created_by` bigint(20) DEFAULT NULL COMMENT '创建人',
  `updated_by` bigint(20) DEFAULT NULL COMMENT '创建人',
  `status` tinyint(4) NOT NULL COMMENT '状态 1、启用 2、停用',
  primary key (`id`) using btree
) engine=innodb default charset=utf8 row_format=dynamic comment='用户信息表'
```
## 5、`主键`优先考虑数字类型的自增型和雪花算法型（建议）
相比于`UUID`数据类型的`主键`性能更好
```sql
create table `audit_log` (
  `id` bigint(20) not null comment '日志主键',
  `created_time` datetime not null comment '创建时间',
  `end_time` datetime not null comment '结束时间',
  primary key (`id`) using btree
) engine=innodb default charset=utf8 row_format=dynamic comment='稽查日志记录'
```
## 6、表、字段前缀命名（建议）
- 表
    - 反例：`t_user`
    - 正例：`sys_user`
> 表的前面没有必要再添加所谓的单个字母的前缀，尤其是`t_`，大家都知道是表，视图可以这么玩。由于以前开发有很多的视图，触发器，存储过程为了方便区分，被数据库支配那几年，
> 想想都恐怖，现在已经不适用了，若是真要添加可以参考以下

 |  前缀+格式   | 说明  | 示例  |
 |  ----  | ----  | ----  |
 | `s_`   | 系统表 | `s_user、s_role`|
 | `d_`   | 字典、码表 | `r_icd_10、r_icd_9、r_dict`|
 | `r_`主表_从表   | 中间表 | `r_user_role`|
 | `b_`维度_业务   | 业务表 | `b_msg_task`|
 
- 字段
    - 反例：为了避免使用数据库关键字和保留字在字段加的前缀，
    - 正列：使用其它代替
> 相比于UUID数据类型的`主键`性能更好
```sql
create table `audit_log` (
  `id` bigint(20) not null comment '日志主键',
  `created_time` datetime not null comment '创建时间',
  `add` datetime not null comment '结束时间',
  primary key (`id`) using btree
) engine=innodb default charset=utf8 row_format=dynamic comment='稽查日志记录'
```