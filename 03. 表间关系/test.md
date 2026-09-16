-- 1. 用户表
CREATE TABLE users (
id SERIAL PRIMARY KEY,
username VARCHAR(50) UNIQUE NOT NULL,
email VARCHAR(100) UNIQUE NOT NULL,
created_at TIMESTAMP DEFAULT now()
);

-- 2. 动态表：多的一方加外键，指向用户
CREATE TABLE posts (
id SERIAL PRIMARY KEY,
user_id INTEGER NOT NULL,
content TEXT NOT NULL,
created_at TIMESTAMP DEFAULT now(),
FOREIGN KEY (user_id) REFERENCES users(id)
);

-- 3. 评论表：同时关联动态和用户（两个外键都是"多"的一方）
CREATE TABLE comments (
id SERIAL PRIMARY KEY,
post_id INTEGER NOT NULL,
user_id INTEGER NOT NULL,
content TEXT NOT NULL,
created_at TIMESTAMP DEFAULT now(),
FOREIGN KEY (post_id) REFERENCES posts(id),
FOREIGN KEY (user_id) REFERENCES users(id)
);

-- 4. 关注表：自关联多对多的中间表
CREATE TABLE follows (
follower_id INTEGER NOT NULL, -- 关注者
followee_id INTEGER NOT NULL, -- 被关注者
created_at TIMESTAMP DEFAULT now(),
PRIMARY KEY (follower_id, followee_id), -- 联合主键，防止重复关注
FOREIGN KEY (follower_id) REFERENCES users(id),
FOREIGN KEY (followee_id) REFERENCES users(id),
CHECK (follower_id <> followee_id) -- 不能自己关注自己
);

-- 5. 标签表
CREATE TABLE tags (
id SERIAL PRIMARY KEY,
name VARCHAR(50) UNIQUE NOT NULL
);

-- 6. 动态-标签中间表：多对多
CREATE TABLE post_tags (
post_id INTEGER NOT NULL,
tag_id INTEGER NOT NULL,
PRIMARY KEY (post_id, tag_id), -- 联合主键
FOREIGN KEY (post_id) REFERENCES posts(id),
FOREIGN KEY (tag_id) REFERENCES tags(id)
);
