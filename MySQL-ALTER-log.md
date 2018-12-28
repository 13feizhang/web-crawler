# 数据库修改记录

![米饭星](http://cdn.mifanxing.com/mifan/img/favicon.ico)
# 2.4.0

### 2018年12月28日
> article,创建nlp_dic_detial_log
```sql
    CREATE TABLE `nlp_dic_detial_log` (
  `id` bigint(10) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `date_node` int(8) unsigned NOT NULL COMMENT '日期',
  `dic_id` bigint(20) unsigned NOT NULL COMMENT '词ID',
  `ssid` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '0' COMMENT '会话标识',
  `category_id` bigint(20) unsigned NOT NULL COMMENT '词典分类',
  `number` int(10) unsigned NOT NULL COMMENT '计数标识',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `date_dic_id_ssid_unique` (`date_node`,`dic_id`,`ssid`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=231 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
> article,创建nlp_dic_search_log
```sql
CREATE TABLE `nlp_dic_search_log` (
  `id` bigint(10) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `date_node` int(8) unsigned NOT NULL COMMENT '日期',
  `search_id` bigint(20) unsigned NOT NULL COMMENT '用户搜索词ID',
  `ssid` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '0' COMMENT '会话标识',
  `number` int(10) unsigned NOT NULL COMMENT '计数标识',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `date_search_id_ssid_unique` (`date_node`,`search_id`,`ssid`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=120 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
> article,创建nlp_dic_search
```sql
CREATE TABLE `nlp_dic_search` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `word` varchar(100) CHARACTER SET utf8 NOT NULL COMMENT '热门词汇',
  `search_num` int(10) unsigned DEFAULT '1' COMMENT '搜索次数',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `word_unique` (`word`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=84 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
> article,创建nlp_dic_search
```sql
CREATE TABLE `nlp_dic_categories` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '词典分类名称',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '使能 0禁止 1启用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=46 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
> article,创建dic_synonym
```sql
CREATE TABLE `dic_synonym` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `dic_id` bigint(20) unsigned NOT NULL COMMENT '词id',
  `synonym` varchar(30) CHARACTER SET utf8 NOT NULL COMMENT '同义词',
  `version` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '版本',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=39 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
> article,创建nlp_event
```sql
CREATE TABLE `nlp_event` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) CHARACTER SET utf8 NOT NULL COMMENT 'nlp事件名称',
  `last_time` datetime NOT NULL,
  `last_id` bigint(20) unsigned NOT NULL,
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### 2018年11月20日
> wxrank.seeds 添加forum_id字段
```sql
ALTER TABLE `seeds`
ADD COLUMN `forum_id`  tinyint NOT NULL DEFAULT 3 AFTER `article_seed_id`;
```

### 2018年11月1日
> wx.topics添加is_repeat字段
```sql
ALTER TABLE `topics`
ADD COLUMN `is_repeat`  tinyint(1) UNSIGNED NOT NULL DEFAULT 0 COMMENT '是否重复，0/1:否/是' AFTER `transmit`;
```
> wx,创建topic_similar
```sql
CREATE TABLE `topic_similar` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `topic_id` bigint(20) unsigned NOT NULL,
  `target_id` bigint(20) unsigned NOT NULL,
  `modified` datetime NOT NULL,
  `created` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_target_unique_idx` (`topic_id`,`target_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
> wx,创建hamming_distances
```sql
CREATE TABLE `hamming_distance` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `distance` int(10) unsigned NOT NULL,
  `theme_id` bigint(20) unsigned NOT NULL,
  `target_id` bigint(20) unsigned NOT NULL,
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `modified` datetime NOT NULL,
  `created` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `theme_id_target_unique` (`theme_id`,`target_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

### 2018年10月19日
> wx新建自定义榜单
```sql
CREATE TABLE `custom_ranks` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '榜单名称',
  `seed_num` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '公众号数量',
  `description` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '描述',
  `reviews` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '阅读数',
  `status` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '公开状态 0:不公开 1:公开',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '使能 0禁止 1启用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


CREATE TABLE `custom_rank_seeds` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `custom_ranks_id` bigint(20) unsigned NOT NULL COMMENT '自定义榜单id',
  `seed_id` bigint(20) unsigned NOT NULL COMMENT '公众号id',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '使能 0禁止 1启用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```


### 2018年9月21日
> article新建移除原因表，新建移除一原因与topic关联表
```sql
CREATE TABLE `topics_abandon` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `topic_id` bigint(20) unsigned NOT NULL COMMENT '文章ID',
  `category_id` bigint(20) unsigned NOT NULL COMMENT '弃用分类ID',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_id_unique` (`topic_id`)
) ENGINE=InnoDB AUTO_INCREMENT=25 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


CREATE TABLE `abandon_categories` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '分类名称',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '使能 0禁止 1启用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### 2018年9月11日
> wxrank.topics 添加关键字字段
```sql
ALTER TABLE `topics`
ADD COLUMN `key_words`  varchar(500) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '关键字' AFTER `content`;
```

### 2018年9月3日
> wxrank.categories 添加新列
```sql
ALTER TABLE `categories`
ADD COLUMN `quotes`  int(10) UNSIGNED NOT NULL DEFAULT 0 COMMENT '公众号引用次数' AFTER `title`,
ADD COLUMN `display_order`  int(10) UNSIGNED NOT NULL DEFAULT 0 COMMENT '排序' AFTER `quotes`;
```
> wxrank.tags 添加新列
```sql
ALTER TABLE `tags`
ADD COLUMN `quotes`  int(10) UNSIGNED NOT NULL DEFAULT 0 COMMENT '公众号引用次数' AFTER `title`,
ADD COLUMN `display_order`  int(10) UNSIGNED NOT NULL DEFAULT 0 COMMENT '排序' AFTER `quotes`;
ADD COLUMN `is_show`  tinyint(1) UNSIGNED NOT NULL DEFAULT 1 COMMENT '1:展示,0:不展示' AFTER `display_order`;
```
> wxrank.topics 添加post_date为索引列
```sql
ALTER TABLE `topics`
ADD INDEX `post_date_index` (`post_date`) ;
```
> wxrank.categories_tags 表新建
```sql
CREATE TABLE `categories_tags` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `category_id` bigint(20) unsigned NOT NULL COMMENT '分类标识',
  `tag_id` bigint(20) unsigned NOT NULL COMMENT '标签标识',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `categoryId_tagId_unique` (`category_id`,`tag_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

### 2018年8月31日
> wxrank.topics 添加transmit
```sql
ALTER TABLE `topics`
ADD COLUMN `transmit `  tinyint(1) UNSIGNED NOT NULL DEFAULT 1 COMMENT '是否传送到artcile中，默认值为1  1为传送  0为不传送' AFTER `group_sign`;
```
### 2018年8月24日
> wxrank.nlp_event 添加表nlp_event,并插入一行数据。
```sql
CREATE TABLE `nlp_event` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) CHARACTER SET utf8 NOT NULL DEFAULT 'nlp事件名称',
  `samples` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '样本数量',
  `result` varchar(100) CHARACTER SET utf8 DEFAULT NULL COMMENT '结果',
  `last_time` datetime NOT NULL,
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ----------------------------
-- Records of nlp_event
-- ----------------------------
INSERT INTO `nlp_event` VALUES ('1', '分词', '0', null, '2018-08-24 10:11:17', '1', '0', '0', '0000-00-00 00:00:00', '2018-08-24 10:11:18');
```
### 2018年8月23日
> wxrank.attachments 添加md5
```sql
ALTER TABLE `attachments`
ADD COLUMN `md5`  varchar(50) CHARACTER SET utf8mb4 NOT NULL DEFAULT '0' AFTER `extra`;
```

> wxrank.attachments_md5 增加一张表
```sql
CREATE TABLE `attachments_md5` (
  `id` bigint(20) NOT NULL,
  `md5` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '不良图片的md5,用于去重',
  PRIMARY KEY (`id`),
  UNIQUE KEY `md5_unique` (`md5`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### 2018年8月6日
> wxrank.seeds 添加subject
```sql
ALTER TABLE `seeds`
ADD COLUMN `添加subject`  varchar(100) DEFAULT NULL COMMENT '帐号主体' AFTER `account_subject`;
```
> wxrank.regions
```sql
ALTER TABLE `regions`
ADD COLUMN `quotes`  int(10) UNSIGNED NOT NULL DEFAULT 0 COMMENT '公众号引用的次数' AFTER `leaf`,
ADD COLUMN `display_order`  int(10) UNSIGNED NOT NULL DEFAULT 0 COMMENT '排序' AFTER `quotes`;
```
> wxrank.tags
```sql
ALTER TABLE `tags`
MODIFY COLUMN `title`  varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '标签名' AFTER `id`,
ADD UNIQUE INDEX `title_unique` (`title`) USING BTREE ;
```
### 2018年7月20日
> article (添加activities,activities_info,posters,poster_activity_images)
```sql
CREATE TABLE `activities` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `title` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '标题',
  `description` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '描述',
  `cover_plan` varchar(255) NOT NULL COMMENT '封面',
  `status` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '活动状态，0:待开始,1:正在进行，2:已结束',
  `start_date` datetime NOT NULL COMMENT '日程开始时间',
  `end_date` datetime NOT NULL COMMENT '日程结束时间',
  `activity_type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '活动类型，0：线上活动；1：线下活动',
  `banner` varchar(255) DEFAULT NULL COMMENT '横幅图片',
  `activity_location` varchar(255) DEFAULT NULL COMMENT '活动地点 用逗号隔开',
  `is_blank` tinyint(1) unsigned NOT NULL COMMENT '是否是外连接 0：否，1：是',
  `content` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci COMMENT '内容',
  `blank` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '来源URL(外链接)',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '启用/禁用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
CREATE TABLE `activities_info` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `activities_id` bigint(20) NOT NULL COMMENT '活动表ID',
  `logo` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT 'logo图',
  `sponsor` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '主办方信息',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
CREATE TABLE `posters` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `title` varchar(100) CHARACTER SET utf8mb4 NOT NULL COMMENT '标题',
  `author` varchar(225) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '作者',
  `cover_plan` varchar(255) NOT NULL COMMENT '封面图',
  `post_time` datetime DEFAULT NULL COMMENT '发布时间',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '启用/禁止',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8;
CREATE TABLE `poster_activity_images` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `type` tinyint(1) NOT NULL COMMENT '图片类型 0：海报图，1：活动图',
  `poster_activity_id` bigint(20) unsigned NOT NULL COMMENT '海报ID\\活动ID',
  `image` varchar(255) DEFAULT NULL COMMENT '图片',
  `description` varchar(255) DEFAULT NULL COMMENT '图片描述',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '启用/禁用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  KEY `FK_poster_activity_image` (`poster_activity_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

```

### 2018年7月10日
> wxrank (添加一个 mobile_wx 表)
```sql
CREATE TABLE `mobile_wx` (
`id`  bigint(20) UNSIGNED NOT NULL AUTO_INCREMENT ,
`mobile_id`  varchar(100) NOT NULL COMMENT '手机唯一标识符' ,
`enabled`  tinyint UNSIGNED NOT NULL DEFAULT 1 COMMENT '使能  0禁止  1启用' ,
`created`  datetime NOT NULL COMMENT '创建时间' ,
`modified`  datetime NOT NULL COMMENT '修改时间' 
);
```
> wxrank.mobile_wx (mobile_id增加唯一索引)
```sql
ALTER TABLE `mobile_wx`
ADD UNIQUE INDEX `mobile_id_unique` (`mobile_id`) USING BTREE ;
```
> wxrank.mobile_wx (增加一个wx_nickname)
```sql
ALTER TABLE `mobile_wx`
ADD COLUMN `wx_nickname`  varchar(100) NOT NULL COMMENT '一个手机一个微信号，这个里面是微信昵称' AFTER `mobile_id`;
```


### 2018年6月26日
> wxrank.seeds (修改地区、分类字段，删除标签字段)
```sql
ALTER TABLE `seeds`
DROP COLUMN `tag`,
DROP COLUMN `region`,
DROP COLUMN `classify`,
ADD COLUMN `region_id`  bigint(20) UNSIGNED NOT NULL DEFAULT 0 COMMENT '地区' AFTER `operating_period`,
ADD COLUMN `category_id`  bigint(20) UNSIGNED NOT NULL DEFAULT 0 COMMENT '分类' AFTER `region_id`;
```
> wxrank (添加分类表categories)
```sql
CREATE TABLE `categories` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(255) NOT NULL COMMENT '公众号分类名',
  `description` varchar(500) DEFAULT NULL COMMENT '分类描述',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;
```
> wxrank (添加标签表tags,公众号和标签关联表seeds_tags)
```sql
CREATE TABLE `tags` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(50) NOT NULL COMMENT '标签名',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `seeds_tags` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `seed_id` bigint(20) unsigned NOT NULL COMMENT '公众号标识',
  `tag_id` bigint(20) unsigned NOT NULL COMMENT '标签标识',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `seed_tag_unique` (`seed_id`,`tag_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 2018年6月21日
> wxrank.topics (topics增加article_topic_id 默认值，增加article_topic_id索引)
```sql
ALTER TABLE `topics`
MODIFY COLUMN `article_topic_id`  bigint(20) UNSIGNED NULL DEFAULT 0 COMMENT '与article topic表中对应id' AFTER `id`,
ADD INDEX `article_topic_id` (`article_topic_id`) USING BTREE ;
```

> wxrank.topics_attachments (topics_attachments增加article_topic_id 默认值0，增加article_topic_id索引)
```sql
ALTER TABLE `topics_attachments`
MODIFY COLUMN `article_topic_id`  bigint(20) UNSIGNED NULL DEFAULT 0 COMMENT '图片发送给article之后，开始更新该表，定期维护' AFTER `topic_id`,
ADD INDEX `article_topic_id` (`article_topic_id`) USING BTREE ;
```




### 2018年6月15日
> wxrank.seeds (添加二维码字段)
```sql
ALTER TABLE `seeds`
ADD COLUMN `qr_code`  varchar(255) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL COMMENT '二维码' AFTER `logo`;
```
> ucenter.activity_rosters (添加活动报名表)
```sql
CREATE TABLE `activity_rosters` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `user_id` bigint(20) unsigned NOT NULL COMMENT '用户标识',
  `phone_number` varchar(11) NOT NULL COMMENT '电话号',
  `full_name` varchar(50) CHARACTER SET utf8mb4 NOT NULL COMMENT '姓名',
  `email` varchar(100) NOT NULL COMMENT '邮箱',
  `city` varchar(50) NOT NULL COMMENT '城市',
  `position` varchar(200) NOT NULL COMMENT '职位',
  `job_nature` varchar(20) NOT NULL COMMENT '工作性质',
  `job_years` varchar(20) NOT NULL COMMENT '工作年限',
  `making_type` varchar(200) NOT NULL COMMENT '制作类型',
  `hope_skill` varchar(20) DEFAULT NULL COMMENT '希望学到的技术',
  `hope_onsite` tinyint(1) unsigned DEFAULT NULL,
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `user_id_unique` (`user_id`),
  UNIQUE KEY `phone_num_unique` (`phone_number`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

> wxrank.regions (添加区域表)
```sql
CREATE TABLE `regions` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `parent_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '父节点',
  `title` varchar(100) NOT NULL COMMENT '区域名称',
  `leaf` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否叶子节点 0：否 ，1：是',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '是否可用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

```

### 2018年6月8日
> wxrank.topics_attachments (topics_attachments增加retry字段)
```sql
ALTER TABLE `topics_attachments`
ADD COLUMN `retry`  int UNSIGNED NOT NULL DEFAULT 0 COMMENT '重试次数，关联关系是否是在article保存次数' AFTER `attachment_id`;
```

### 2018年5月29日
> wx(微信星榜初始化)
```sql
CREATE TABLE `wetchat_mobile_log` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `mobile_id` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '手机唯一标识符',
  `state` tinyint(4) unsigned DEFAULT NULL COMMENT '状态',
  `script_no` varchar(50) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '脚本编号（是那个脚本执行的）',
  `content` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '内容  （开始执行什么了）',
  `created` datetime NOT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


CREATE TABLE `wechat_comment` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `super_id` bigint(20) unsigned DEFAULT '0' COMMENT '评论的父id',
  `topic_id` bigint(20) unsigned NOT NULL COMMENT '主题id',
  `comment_thumbs_up` bigint(20) DEFAULT NULL COMMENT '评论点赞数',
  `comment_content` text COLLATE utf8mb4_unicode_ci COMMENT '评论内容',
  `comment_author` varchar(50) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '评论作者',
  `enabled` tinyint(4) unsigned DEFAULT '1',
  `creator` bigint(4) unsigned DEFAULT '0' COMMENT '创建人',
  `created` datetime DEFAULT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


CREATE TABLE `topics_fetch` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `topic_id` bigint(20) unsigned NOT NULL COMMENT '主题ID',
  `seed_id` bigint(20) unsigned NOT NULL COMMENT '种子ID',
  `origin` varchar(1000) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '来源URL  让以后再次访问使用',
  `origin_hash` bigint(20) NOT NULL DEFAULT '0' COMMENT '来源URL HASH',
  `reviews` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '阅读数',
  `thumbs_up` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '点赞数',
  `comment_total` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '回复数（评论数）',
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_id_idx` (`topic_id`) USING BTREE,
  KEY `seed_id_idx` (`seed_id`) USING BTREE,
  KEY `origin_hash_idx` (`origin_hash`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `topics_attachments` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `topic_id` bigint(20) unsigned NOT NULL COMMENT '主题ID',
  `article_topic_id` bigint(20) unsigned DEFAULT NULL COMMENT '图片发送给article之后，开始更新该表，定期维护',
  `attachment_id` bigint(20) unsigned NOT NULL COMMENT '附件ID',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '启用/禁用',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_id_attachment_id_unique` (`topic_id`,`attachment_id`) USING BTREE,
  KEY `attachment_id_idx` (`attachment_id`) USING BTREE,
  KEY `topic_id_enabled_attachment_id_idx` (`topic_id`,`enabled`,`attachment_id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `topics` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `article_topic_id` bigint(20) unsigned DEFAULT NULL COMMENT '与article topic表中对应id',
  `seed_id` bigint(20) unsigned NOT NULL,
  `type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '主题类型(0：图文，1：视频，2：音频，3：音视频混合)',
  `title` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '标题',
  `title_hash` bigint(20) NOT NULL DEFAULT '0' COMMENT '标题HASH',
  `content` mediumtext CHARACTER SET utf8mb4 NOT NULL COMMENT '内容',
  `author` varchar(255) CHARACTER SET utf8mb4 DEFAULT NULL COMMENT '作者',
  `post_date` datetime NOT NULL COMMENT '发布日期',
  `top` tinyint(4) unsigned NOT NULL DEFAULT '0' COMMENT '是否头条{1：是，0：否} ',
  `original` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否原创 0：否，1：是',
  `group_sign` bigint(20) unsigned NOT NULL COMMENT '分组标记',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '启用/禁用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  KEY `creator_enabled_modified_idx` (`creator`,`enabled`,`modified`) USING BTREE,
  KEY `modified_idx` (`modified`) USING BTREE,
  KEY `title_hash_idx` (`title_hash`) USING BTREE,
  KEY `forum_id_train_sample_idx` (`type`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `seeds` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'ID 自增长',
  `article_seed_id` bigint(20) unsigned DEFAULT NULL COMMENT 'seed 与article的seed表 id对应表',
  `title` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '微信号的名称',
  `wechat_id` varchar(200) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '微信号wechat_id',
  `biz` varchar(20) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '公众号唯一识别符(唯一)',
  `logo` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '公众号logo',
  `func_intro` text COLLATE utf8mb4_unicode_ci COMMENT '功能介绍   ',
  `account_type` tinyint(3) unsigned DEFAULT NULL COMMENT '订阅号0   服务号1',
  `account_subject` tinyint(1) DEFAULT NULL COMMENT '企业   和    个人',
  `company_name` varchar(200) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '企业全称',
  `business_reg_no` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '工商执照注册号',
  `service_tel` varchar(50) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '客服电话',
  `company_type` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '企业类型',
  `business_scope` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '经营范围',
  `establish_date` date DEFAULT NULL COMMENT '企业成立日期',
  `operating_period` date DEFAULT NULL COMMENT '企业经营期限',
  `region` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '地区',
  `classify` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '分类',
  `tag` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '标签，每个标签使用逗号隔开',
  `description` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '描述',
  `update_rate` int(10) DEFAULT NULL COMMENT '更新频率',
  `language` tinyint(1) DEFAULT NULL COMMENT '0中文  1英文',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '使能  0禁止  1启用',
  `creator` bigint(20) unsigned DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


CREATE TABLE `scheduled_job` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `version` int(11) NOT NULL DEFAULT '0' COMMENT '版本号',
  `job_status` enum('NORMAL','EXCEPTIONAL','DONE') CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'DONE' COMMENT '任务状态',
  `start_time` datetime DEFAULT NULL COMMENT '任务开始时间',
  `end_time` datetime DEFAULT NULL COMMENT '任务结束时间',
  `last_modified_time` datetime DEFAULT NULL COMMENT '最后更新时间',
  `message` text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci COMMENT '异常信息',
  `description` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '任务描述',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;


CREATE TABLE `rank` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '类型，0：日榜 1:周榜 2：月榜',
  `seed_id` bigint(20) unsigned NOT NULL COMMENT '来源标识',
  `start_date` int(8) unsigned NOT NULL COMMENT '开始日期',
  `end_date` int(8) unsigned NOT NULL COMMENT '结束日期',
  `post_num` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '发布次数',
  `topic_num` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '文章数量',
  `all_reviews` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '总阅读数',
  `top_reviews` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '头条阅读数',
  `max_reviews` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最高阅读数',
  `all_praises` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '总点赞数',
  `top_praises` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '头条点赞数',
  `max_praises` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最高点赞数',
  `star_index` double(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '米饭星指数',
  `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '是否可用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `seed_type_start_unique` (`type`,`seed_id`,`start_date`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `attachments_fetch` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `attachment_id` bigint(20) unsigned NOT NULL COMMENT '附件ID',
  `origin` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '来源URL',
  `origin_hash` bigint(20) NOT NULL COMMENT '来源URL HASH',
  PRIMARY KEY (`id`),
  KEY `attachment_id_idx` (`attachment_id`) USING BTREE,
  KEY `origin_hash_idx` (`origin_hash`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `attachments` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `mime` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT 'MIME类型',
  `filename` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '附件名称',
  `extension` varchar(10) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '扩展名称',
  `title` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '附件标题',
  `description` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '附件描述',
  `extra` varchar(1024) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '额外信息',
  `filesize` int(11) NOT NULL DEFAULT '0' COMMENT '附件大小',
  `download` int(11) NOT NULL DEFAULT '0' COMMENT '下载次数',
  `retry` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '重试次数',
  `group_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '分组  0为不放入封面的图片，1为可放入文章图片关联',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '启用/禁用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  KEY `enabled_retry_idx` (`enabled`,`retry`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8;




```

### 2018年5月28日
> wxrank.seeds (seeds增加logo字段)
```sql
ALTER TABLE `seeds`
ADD COLUMN `logo`  varchar(255) NULL COMMENT '公众号logo'  AFTER `biz`;
```


### 2018年5月25日
> wxrank.topics_attachments (topics_attachments增加article_topic_id字段)
```sql
ALTER TABLE `topics_attachments`
ADD COLUMN `article_topic_id`  bigint UNSIGNED NULL COMMENT '图片发送给article之后，开始更新该表，定期维护' AFTER `topic_id`;
```

### 2018年5月23日
> article.seeds (seeds增加wx字段)，article.topics_document (topics_document增加type、original)
```sql
ALTER TABLE `mifan_article`.`seeds`
ADD COLUMN `wx` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '区分： 0:非公众号  1：公众号';

ALTER TABLE `mifan_article`.`topics_document`
ADD COLUMN `type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '主题类型(0：图文，1：视频，2：音频，3：音视频混合)',
ADD COLUMN `original` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否原创 0：否，1：是';
```

### 2018年5月21日
> wxrank.seeds (seeds增加article_seed_id字段)，wxrank.topics (topics增加article_topic_id)
```sql
ALTER TABLE `mifan_wxrank`.`seeds`
ADD COLUMN `article_seed_id` bigint unsigned  COMMENT 'seed 与article的seed表 id对应表';

ALTER TABLE `mifan_wxrank`.`topics`
ADD COLUMN `article_topic_id` bigint unsigned  COMMENT '与article topic表中对应id';
```

---
# 2.3.0
### 2018年4月18日
> article(重建封面图表)
```sql
CREATE TABLE `cover_images` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `topic_id` bigint(20) unsigned NOT NULL,
  `attachment_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '关联attachments表',
  `display_order` int(10) NOT NULL DEFAULT '0' COMMENT '排序',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_id_att_id_unique` (`topic_id`,`attachment_id`) USING BTREE,
  KEY `topic_id_enabled_idx` (`topic_id`,`enabled`)
) ENGINE=InnoDB AUTO_INCREMENT=27 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


```
### 2018年4月13日
> article(新建短链接表和封面图表)
```sql
CREATE TABLE `short_urls` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `topic_id` bigint(20) unsigned NOT NULL,
  `origin` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `reviews` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '浏览次数',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_idx` (`topic_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1474 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `cover_images` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `topic_id` bigint(20) unsigned NOT NULL,
  `attachment_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '关联attachments表',
  `image` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '图片地址',
  `title` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '图标题',
  `display_order` int(10) NOT NULL DEFAULT '0' COMMENT '排序',
  `extension` varchar(10) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '拓展名',
  `filesize` int(11) NOT NULL DEFAULT '0' COMMENT '大小',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `topic_id_enabled_idx` (`topic_id`,`enabled`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


```

### 2018年3月29日
> article(新建美频的6张表)
```sql
CREATE TABLE `mp_brands` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '品牌名',
  `description` varchar(500) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `feature` varchar(500) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `image` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL,
  `modifier` bigint(20) unsigned NOT NULL,
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
CREATE TABLE `mp_categories` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `root_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '根节点',
  `parent_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '父节点',
  `type` tinyint(1) NOT NULL COMMENT '0:大类,1:型号,2:小类',
  `title` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '标题',
  `image` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '标题图',
  `mobile_image` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '手机标题图',
  `path` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '路径',
  `depth` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '深度',
  `leaf` tinyint(1) NOT NULL DEFAULT '0' COMMENT '叶子节点',
  `display_order` int(11) NOT NULL DEFAULT '0' COMMENT '排序',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
CREATE TABLE `mp_brand_categories` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `brand_id` bigint(20) unsigned NOT NULL COMMENT '品牌标识',
  `mp_category_id` bigint(20) unsigned NOT NULL COMMENT '分类标识',
  `type` tinyint(1) NOT NULL DEFAULT '0' COMMENT '0:大类,1:型号',
  PRIMARY KEY (`id`),
  KEY `brand_type_idx` (`brand_id`,`type`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
CREATE TABLE `mp_downloads` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `type` tinyint(1) NOT NULL COMMENT '0：91助手，1：驱动下载，2：常用软件',
  `display_order` int(10) NOT NULL DEFAULT '0' COMMENT '排序',
  `title` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '标题',
  `description` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '描述',
  `image` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '标题图',
  `link` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '下载地址',
  `times` int(10) NOT NULL DEFAULT '0' COMMENT '下载次数',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否可用',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `created` datetime NOT NULL COMMENT '创建时间',
  `modified` datetime NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  KEY `type_index` (`type`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
CREATE TABLE `topics_mp` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `type` tinyint(1) NOT NULL COMMENT '类型：0：图文，1：视频',
  `topic_id` bigint(20) unsigned NOT NULL,
  `up_times` int(10) NOT NULL DEFAULT '0' COMMENT 'up次数',
  `mp_category_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '美频分类',
  PRIMARY KEY (`id`),
  UNIQUE KEY `topics_idx_unique` (`topic_id`) USING BTREE,
  KEY `mp_category_idx` (`mp_category_id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
CREATE TABLE `topics_mpdownloads` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `topic_id` bigint(20) unsigned NOT NULL,
  `mp_download_id` bigint(20) unsigned NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_download_unique` (`topic_id`,`mp_download_id`) USING BTREE,
  KEY `topic_idx` (`topic_id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```


# 2.2.0

### 2018年3月14日
> article.topics_product (topics_product表增加一个销售排名字段)
```sql
ALTER TABLE `mifan_article`.`topics_product`
ADD COLUMN `sale_rank`  bigint(20)  UNSIGNED ;
```

### 2018年3月9日
> mifan-article (新建表)
```sql
CREATE TABLE `user_search_history` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `ssid` char(36) COLLATE utf8mb4_unicode_ci NOT NULL,
  `forum_id` bigint(20) unsigned NOT NULL DEFAULT '0',
  `category_id` bigint(20) unsigned NOT NULL DEFAULT '0',
  `user_id` bigint(20) unsigned NOT NULL DEFAULT '0',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `modified` datetime NOT NULL,
  `created` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `ssid_forum_category_unique` (`ssid`,`forum_id`,`category_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=10024 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='用户搜索记录';

CREATE TABLE `search_keyword` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `search_id` bigint(20) unsigned NOT NULL COMMENT 'user_search_history FK',
  `keyword` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `num` int(10) NOT NULL DEFAULT '1' COMMENT '次数',
  `enabled` tinyint(4) NOT NULL DEFAULT '1',
  `modified` datetime NOT NULL,
  `created` datetime NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1273 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='搜索关键字';

```

### 2018年2月11日
> mifan-quiz (新建表)
```sql
CREATE TABLE `quizs` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `title` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '标题',
  `description` varchar(255) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '描述',
  `back_img` varchar(1000) COLLATE utf8mb4_unicode_ci DEFAULT NULL COMMENT '背景图',
  `state` tinyint(1) NOT NULL DEFAULT '0' COMMENT '0：待发布，1：发布中，2，结束发布',
  `question_num` int(10) NOT NULL DEFAULT '0' COMMENT '题目个数',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否可用',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `modified` datetime NOT NULL COMMENT '修改时间',
  `created` datetime NOT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=23 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `questions` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `quiz_id` bigint(20) unsigned NOT NULL COMMENT '问卷id',
  `question_title` varchar(500) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '问题标题',
  `type` tinyint(1) NOT NULL COMMENT '类型，1：单选 2：多选',
  `display_order` int(10) NOT NULL DEFAULT '1' COMMENT '排序',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否可用',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modified` datetime NOT NULL COMMENT '修改时间',
  `created` datetime NOT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=72 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `options` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `question_id` bigint(20) unsigned NOT NULL COMMENT '问题ID',
  `option_title` varchar(500) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '选项标题',
  `is_correct` tinyint(1) NOT NULL COMMENT '是否正确选项,0:否  1:是',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否可用',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modified` datetime NOT NULL COMMENT '修改时间',
  `created` datetime NOT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=238 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `answers` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `session_id` bigint(20) unsigned NOT NULL,
  `question_id` bigint(20) unsigned NOT NULL COMMENT '问题标识',
  `answers` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT '用户提交的答案',
  `is_right` tinyint(1) NOT NULL COMMENT '是否答对 0：否  1：是',
  `modified` datetime NOT NULL COMMENT '修改时间',
  `created` datetime NOT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `session_id_question_id_unique` (`session_id`,`question_id`)
) ENGINE=InnoDB AUTO_INCREMENT=2421 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `quiz_session` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `session_code` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL COMMENT 'session编码',
  `quiz_id` bigint(20) unsigned NOT NULL COMMENT '试卷ID',
  `answer_num` int(10) NOT NULL DEFAULT '0' COMMENT '答题个数',
  `right_num` int(10) NOT NULL DEFAULT '0' COMMENT '答对个数',
  `all_done` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否全部答完 0：否  1：是',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '修改人',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '创建人',
  `modified` datetime NOT NULL COMMENT '修改时间',
  `created` datetime NOT NULL COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1270 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

CREATE TABLE `quiz_count` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `quiz_id` bigint(20) unsigned NOT NULL,
  `peoples` int(10) NOT NULL DEFAULT '0' COMMENT '完成人数',
  `first` int(10) NOT NULL DEFAULT '0' COMMENT '得分0-10%人数',
  `second` int(10) NOT NULL DEFAULT '0' COMMENT '得分10-19%人数',
  `third` int(10) NOT NULL DEFAULT '0' COMMENT '得分20-29%人数',
  `fourth` int(10) NOT NULL DEFAULT '0' COMMENT '得分30-39%人数',
  `fifth` int(10) NOT NULL DEFAULT '0' COMMENT '得分40-49%人数',
  `sixth` int(10) NOT NULL DEFAULT '0' COMMENT '得分50-59%人数',
  `seventh` int(10) NOT NULL DEFAULT '0' COMMENT '得分60-69%人数',
  `eighth` int(10) NOT NULL DEFAULT '0' COMMENT '得分70-79%人数',
  `ninth` int(10) NOT NULL DEFAULT '0' COMMENT '得分80-89%人数',
  `tenth` int(10) NOT NULL DEFAULT '0' COMMENT '得分90-100%人数',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `modified` datetime NOT NULL,
  `created` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `count_quiz_id_unique` (`quiz_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1395 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;


```

### 2018年1月31日
> article.topics_fetch (topics_fetch表增加一个点赞数字段)
```sql
ALTER TABLE `mifan_article`.`topics_fetch`
ADD COLUMN `thumbs_up`  int(10) UNSIGNED  NOT NULL DEFAULT 0  AFTER `reviews`;
```

### 2018年1月26日

> article.system_resource_lock (article服务的全局资源锁表)
```sql
CREATE TABLE `mifan_article`.`system_resource_lock` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  `resource_type` TINYINT UNSIGNED NOT NULL,
  `target_id` BIGINT UNSIGNED NOT NULL,
  `version` INT NOT NULL DEFAULT 1,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `resource_type_target_id_unique` (`resource_type` ASC, `target_id` ASC),
  INDEX `resource_type_target_id_version_idx` (`resource_type` ASC, `target_id` ASC, `version` ASC));
```

---

# 2.2.0

### 2018年1月10日

> support.event_log
```sql
ALTER TABLE `mifan_support`.`event_log`
ADD COLUMN `enabled`  tinyint(1) UNSIGNED NOT NULL DEFAULT 1 AFTER `ip`;
ADD COLUMN `version`  vachar(50) UNSIGNED AFTER `params`;
ADD COLUMN `pc_mobile`  tinyint(1) UNSIGNED AFTER `version`;
```

### 2017年12月14日

> article.folders 目录表
```sql
CREATE TABLE `folders` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `parent_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT 'parent ID',
  `folder_type` tinyint(4) NOT NULL DEFAULT '0',
  `folder_name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '文件夹名称',
  `amount` int(11) NOT NULL DEFAULT '0' COMMENT '当前大小',
  `capacity` int(11) NOT NULL DEFAULT '6' COMMENT '最大容量',
  `display_order` int(11) NOT NULL DEFAULT '0',
  `cancellable` tinyint(1) NOT NULL DEFAULT '1',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `parent_id_idx` (`parent_id`),
  KEY `enabled_creator_folder_type_parent_id_display_order_idx` (`enabled`,`creator`,`folder_type`,`parent_id`,`display_order`),
  KEY `enabled_creator_folder_type_display_order_idx` (`enabled`,`creator`,`folder_type`,`display_order`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

> article.users_topics_compare 用户添加主题比较的关联数据表
```sql
CREATE TABLE `users_topics_compare` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `user_id` bigint(20) unsigned NOT NULL,
  `topic_id` bigint(20) unsigned NOT NULL,
  `folder_id` bigint(20) unsigned NOT NULL,
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `user_id_topic_id_idx` (`user_id`,`topic_id`,`folder_id`),
  KEY `topic_id_idx` (`topic_id`),
  KEY `user_id_enabled_topic_id_idx` (`user_id`,`enabled`,`topic_id`),
  KEY `user_id_enabled_folder_id_topic_id_idx` (`user_id`,`enabled`,`folder_id`,`topic_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

---

# 2.1.0

### 2017年11月25日 2.1.0 数据库改动 

> article.quartz_jobs 任务表 
```sql
CREATE TABLE `quartz_jobs` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `job_status` enum('NEW','SUSPEND','RUNNABLE','TERMINATED') NOT NULL DEFAULT 'NEW' COMMENT '任务状态',
  `job_group` varchar(100) NOT NULL DEFAULT 'DEFAULT' COMMENT '任务组',
  `job_name` varchar(100) NOT NULL COMMENT '任务名称',
  `job_class` varchar(255) NOT NULL COMMENT '任务类',
  `job_data` text COMMENT '任务参数, JSON格式',
  `job_data_template` text COMMENT '任务参数模板',
  `trigger_group` varchar(100) NOT NULL DEFAULT 'DEFAULT' COMMENT '触发器组',
  `trigger_name` varchar(100) NOT NULL COMMENT '触发器名称',
  `trigger_cron_expression` varchar(255) NOT NULL COMMENT 'CRON表达式',
  `description` varchar(255) DEFAULT NULL COMMENT '任务描述',
  `message` text,
  `start_time` datetime DEFAULT NULL,
  `end_time` datetime DEFAULT NULL,
  `last_start_time` datetime DEFAULT NULL,
  `version` int(11) NOT NULL DEFAULT '0' COMMENT '版本号',
  `auto` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否自动启动',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `enabled_auto_idx` (`enabled`,`auto`),
  KEY `job_status_idx` (`job_status`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 2017年11月24日 2.1.0 数据库改动

> support.event_dic，增加enabled字段
```sql
ALTER TABLE `mifan_support`.`event_dic`
ADD COLUMN `enabled`  tinyint(1) UNSIGNED NOT NULL DEFAULT 1 AFTER `event_describe`;
```

### 2017年11月23日 2.1.0 数据库改动

> article.scheduled_job，增加一行数据（排行榜定时任务）
```sql
INSERT INTO `scheduled_job` VALUES (5, 0, 'DONE', '2017-11-23 14:42:23', '2017-11-23 14:42:26', '2017-11-23 14:42:30', NULL, '排行榜定时', 1);
```

### 2017年11月21日 2.1.0 数据库改动

> article.channels，增加频道表创建人和修改人字段
```sql
ALTER TABLE `mifan_article`.`channels`
ADD COLUMN `creator`  bigint(20) UNSIGNED NOT NULL DEFAULT 0 AFTER `enabled`,
ADD COLUMN `modifier`  bigint(20) UNSIGNED NOT NULL DEFAULT 0 AFTER `creator`;
```
> article.channels，增加新字段
```sql
ALTER TABLE `mifan_article`.`channels` 
ADD COLUMN `channel_source` TINYINT UNSIGNED NOT NULL DEFAULT 0 COMMENT '频道来源 0:默认, 1:国内频道, 2:国外频道' AFTER `target_id`,
ADD COLUMN `display_order` INT NOT NULL DEFAULT 0 COMMENT '排序' AFTER `watched`,
ADD INDEX `enabled_channel_source_display_order_idx` (`enabled` ASC, `channel_source` ASC, `display_order` ASC);
```

### 2017年11月02日 2.1.0 数据库改动

> article.translate_task，新增翻译任务表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`translate_task` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `topic_id` bigint(20) unsigned NOT NULL,
  `post_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '翻译文档关联id',
  `state` tinyint(3) unsigned NOT NULL COMMENT '状态：1：待领取，2：未提交，3：待审核，4：审核中，5：审核失败，6：审核成功，7：已支付',
  `words_num` int(11) DEFAULT NULL COMMENT '单词数',
  `words_num_cn` int(11) DEFAULT NULL COMMENT '中文字数',
  `bonus` decimal(10,2) DEFAULT NULL COMMENT '支付金额',
  `translator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '翻译人',
  `auditor` bigint(20) NOT NULL DEFAULT '0',
  `audit_opinion` text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci COMMENT '审核员反馈',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '启用/禁用',
  `modifier` bigint(20) unsigned NOT NULL COMMENT '修改人',
  `creator` bigint(20) unsigned NOT NULL COMMENT '创建人',
  `modified` datetime NOT NULL COMMENT '修改时间',
  `created` datetime NOT NULL COMMENT '创建人',
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_id_unique` (`topic_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=34 DEFAULT CHARSET=utf8;
```

### 2017年11月08日 2.1.0 数据库改动

> article.topics_model新增表, 分类模型增加数据库表, 修改表, 增加了一个字段
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`topics_model` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `forum_id` bigint(20) unsigned NOT NULL COMMENT '版块ID',
  `model_status` enum('NEW','RUNNABLE','CLASSIFICATION','WAITING','DONE','DELETE','TERMINATED') NOT NULL DEFAULT 'NEW' COMMENT 'NEW:新创建的模型\nRUNNABLE:正在执行训练的模型\nCLASSIFICATION:正在执行全量分类\nWAITING:等待\nDONE:完成\nDELETE:已经被删除, 无法回滚\nTERMINATED:训练中的模型被终止',
  `model_name` varchar(100) NOT NULL COMMENT '模型名称, e.g : topic-1',
  `priority` tinyint(4) NOT NULL DEFAULT '0' COMMENT '每个forum_id下, 使用一个优先级最高的模型用于分类',
  `conf_random_selection` tinyint(4) NOT NULL DEFAULT '20' COMMENT '(1-100) n%用于测试数据',
  `conf_overwrite` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否覆盖',
  `result_samples` int(11) NOT NULL DEFAULT '0' COMMENT '训练样本数量',
  `result_text` text COMMENT '模型结果:测试结果或异常结果',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `enabled_forum_id_model_status_priority_idx` (`enabled`,`forum_id`,`model_status`,`priority`),
  KEY `forum_id_model_status_priority_idx` (`forum_id`,`model_status`,`priority`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 2017年10月31日 2.1.0 数据库改动

> article.topics表改动 增加两个新字段, 修改索引, SQL初始化训练数据
```sql
ALTER TABLE `mifan_article`.`topics` 
ADD COLUMN `boost` DOUBLE NOT NULL DEFAULT 1.0 AFTER `thumbs_down`,
ADD COLUMN `train_sample` TINYINT(1) NOT NULL DEFAULT 0 AFTER `boost`,
ADD INDEX `forum_id_train_sample_idx` (`forum_id` ASC, `train_sample` ASC);
```
```sql
ALTER TABLE `mifan_article`.`topics` 
DROP INDEX `forum_id_idx` ;
```
```sql
## 初始化训练样本集合, 使用thomman的数据
UPDATE topics, topics_fetch SET topics.train_sample = 1 
WHERE topics.id = topics_fetch.topic_id AND topics_fetch.seed_id = 11;

## 将Sheet分类从训练样本中删除
update topics, topics_fetch, posts, posts_text
set topics.train_sample = 0
where topics_fetch.topic_id = topics.id and topics_fetch.seed_id = 11
and posts.topic_id = topics.id
and posts_text.id = posts.id and posts_text.category regexp '^Sheet';

## 将Computer Audio分类加入到训练样本中
update topics, topics_classification
set topics.train_sample = 1
where topics_classification.id = topics.id 
and topics_classification.target_variable regexp '^\\[\\{"en":"Computer Audio"';
```
> article.topics表改动 增加人工标注的字段 用于与标注的类别进行关联
```sql
ALTER TABLE `mifan_article`.`topics` 
ADD COLUMN `category_id` BIGINT UNSIGNED NOT NULL DEFAULT 0 AFTER `forum_id`,
ADD INDEX `category_id_idx` (`category_id` ASC);
```
> article.forum_categories 新增类别表, 初始化数据见mifan-api项目的data-categories.sql文件
```sql
CREATE TABLE `forum_categories` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `forum_id` bigint(20) unsigned NOT NULL COMMENT '版块ID',
  `root_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '根节点ID',
  `parent_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '父节点ID',
  `title` varchar(255) DEFAULT NULL,
  `filename` varchar(255) NOT NULL DEFAULT '' COMMENT '分类图片地址',
  `path` varchar(255) DEFAULT NULL,
  `depth` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '深度',
  `leaf` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否是叶子节点',
  `display_order` int(11) NOT NULL DEFAULT '0',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `root_id_enabled_idx` (`root_id`,`enabled`),
  KEY `parent_id_enabled_idx` (`parent_id`,`enabled`),
  KEY `enabled_forum_id_root_id_parent_id_idx` (`enabled`,`forum_id`,`root_id`,`parent_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
> article.word_dictionary 英汉字典表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`word_dictionary` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  `word_en` VARCHAR(255) NOT NULL,
  `word_en_hash` BIGINT NOT NULL,
  `word_cn` VARCHAR(255) NOT NULL,
  `word_cn_hash` BIGINT NOT NULL,
  `enabled` TINYINT(1) NOT NULL DEFAULT 1,
  `creator` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `modifier` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `created` DATETIME NOT NULL,
  `modified` DATETIME NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `enabled_word_en_hash_idx` (`enabled` ASC, `word_en_hash` ASC),
  INDEX `enabled_word_cn_hash_idx` (`enabled` ASC, `word_cn_hash` ASC))
ENGINE = InnoDB
```

### 2017年9月27日 2.0.3 数据库改动

> article.navigation 新增导航菜单表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`navigation` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `parent_id` BIGINT UNSIGNED NOT NULL DEFAULT 0 COMMENT 'PARENT ID',
  `elastic_query_builder_id` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `display_order` INT NOT NULL DEFAULT 0 COMMENT 'display order',
  `title` VARCHAR(100) NULL COMMENT '标题',
  `image` VARCHAR(255) NULL,
  `enabled` TINYINT(1) NOT NULL DEFAULT 1,
  `creator` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `modifier` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `created` DATETIME NOT NULL,
  `modified` DATETIME NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `elastic_query_builder_id_idx` (`elastic_query_builder_id` ASC),
  INDEX `parent_id_idx` (`parent_id` ASC),
  INDEX `enabled_parent_id_display_order_idx` (`enabled` ASC, `parent_id` ASC, `display_order` ASC))
ENGINE = InnoDB;
```
> article.elastic_query_builder 新增elastic查询表
```sql
CREATE TABLE `elastic_query_builder` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(100) NOT NULL COMMENT '查询建造器名称',
  `query_fields_en` varchar(255) DEFAULT NULL COMMENT '查询英文字段',
  `query_fields_cn` varchar(255) DEFAULT NULL COMMENT '查询中文字段',
  `filter_categories_en` varchar(255) DEFAULT NULL COMMENT '过滤器英文类别字段',
  `filter_categories_cn` varchar(255) DEFAULT NULL COMMENT '过滤器中文类别字段',
  `positive_query_string_en` varchar(1024) DEFAULT NULL COMMENT '正相关英文查询字符串',
  `positive_query_string_cn` varchar(1024) DEFAULT NULL COMMENT '正相关中文查询字符串',
  `negative_query_string_en` varchar(1024) DEFAULT NULL COMMENT '负相关英文查询字符串',
  `negative_query_string_cn` varchar(1024) DEFAULT NULL COMMENT '负相关中文查询字符串',
  `negative_boost` double NOT NULL DEFAULT '0.2' COMMENT '负助推因子',
  `function_score_mode` enum('FIRST','MULTIPLY','SUM','AVG','MAX','MIN') NOT NULL DEFAULT 'MULTIPLY' COMMENT '函数评分模式',
  `function_boost_mode` enum('REPLACE','MULTIPLY','SUM','AVG','MAX','MIN') NOT NULL DEFAULT 'MULTIPLY' COMMENT '函数助推模式',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `enabled_idx` (`enabled`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
> article.elastic_function_score 新增elastic函数评分表
```sql
CREATE TABLE `elastic_function_score` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(100) NOT NULL COMMENT '助推函数名称/标题',
  `numeric_field` varchar(100) NOT NULL COMMENT '搜索引擎映射中用于助推的数字型字段名称/路径',
  `filters` varchar(255) DEFAULT NULL COMMENT '过滤器条件, 格式:{field:value}',
  `weight` double NOT NULL DEFAULT '1' COMMENT '权重',
  `function_modifier` enum('NONE','LOG','LOG1P','LOG2P','LN','LN1P','LN2P','SQUARE','SQRT','RECIPROCAL') NOT NULL DEFAULT 'NONE' COMMENT '函数修饰',
  `function_factor` double NOT NULL DEFAULT '1' COMMENT '函数因子',
  `function_missing` double NOT NULL DEFAULT '1' COMMENT '部分文档缺失特定field时使用此值作为默认值',
  `function_global` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否影响全局搜索 1:是, 0:否',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `enabled_global_idx` (`enabled`,`function_global`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 2017年9月15日 

> ucenter.user_addresses 新增字段
```sql
ALTER TABLE `ucenter`.`user_addresses` 
ADD COLUMN `address_label` VARCHAR(255) NULL COMMENT '地址标签' AFTER `address`,
ADD COLUMN `postal_code` VARCHAR(30) NULL COMMENT '邮政编码' AFTER `address_label`;
```

### 2017年9月12日 

> mifan_article.moderation 删除原post_id_idx普通索引
```sql
ALTER TABLE `mifan_article`.`moderation` 
DROP INDEX `post_id_idx` ;
```

> mifan_article.moderation 设置其为唯一索引
```sql
ALTER TABLE `mifan_article`.`moderation` 
ADD UNIQUE INDEX `post_id_unique` (`post_id` ASC);
```

> mifan_article.hope_translate 设置联合唯一索引
```sql
ALTER TABLE `mifan_article`.`hope_translate` 
ADD UNIQUE INDEX `user_id_extend_id_unique` (`user_id` ASC, `extend_id` ASC);
```

> mifan_support.praise 设置默认值为0, 不允许为空
```sql
ALTER TABLE `mifan_support`.`praise` 
CHANGE COLUMN `comment_id` `comment_id` BIGINT(20) UNSIGNED NOT NULL DEFAULT '0' COMMENT '评论标识' ;
```

>  mifan_support.praise 删除comment_id外键
```sql
ALTER TABLE `mifan_support`.`praise` 
DROP INDEX `comment_id` ;
```

>  mifan_support.praise 设置联合唯一索引
```sql
ALTER TABLE `mifan_support`.`praise` 
ADD UNIQUE INDEX `conf_id_theme_id_comment_id_creator_unique` (`conf_id` ASC, `theme_id` ASC, `comment_id` ASC, `creator` ASC);
```

### 2017年10月28日 

> mifan_article.topics_fetch 修改原topic_id_idx普通索引为唯一索引
```sql
ALTER TABLE `mifan_article`.`topics_fetch` 
DROP INDEX `topic_id_idx` ;
ADD UNIQUE INDEX `topic_id_idx` (`topic_id` ASC);
```
