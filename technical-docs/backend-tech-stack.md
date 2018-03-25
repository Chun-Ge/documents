# 1.database scheme

```sql


-- tables
-- Table: Category
CREATE TABLE Category (
    category_name varchar(200) NOT NULL,
    Category_ID int NOT NULL,
    CONSTRAINT Category_pk PRIMARY KEY (Category_ID)
);

-- Table: Comment
CREATE TABLE Comment (
    Comment_ID int NOT NULL,
    Post int NOT NULL,
    content text NOT NULL,
    author int NOT NULL,
    random_name int NOT NULL,
    publish_date time NOT NULL,
    likes int NOT NULL,
    CONSTRAINT Comment_ID PRIMARY KEY (Comment_ID)
);

-- Table: Name_Lib
CREATE TABLE Name_Lib (
    name_ID int NOT NULL,
    topic int NOT NULL,
    name int NOT NULL,
    CONSTRAINT Name_Lib_pk PRIMARY KEY (name_ID)
);

-- Table: Post
CREATE TABLE Post (
    Post_ID int NOT NULL,
    category int NOT NULL,
    author int NOT NULL,
    random_name int NOT NULL,
    content text NOT NULL,
    publish_date time NOT NULL,
    likes int NOT NULL,
    CONSTRAINT Post_pk PRIMARY KEY (Post_ID)
);

-- Table: Topic
CREATE TABLE Topic (
    Topic_ID int NOT NULL,
    Topic_name varchar(200) NOT NULL,
    description text NULL,
    CONSTRAINT Topic_pk PRIMARY KEY (Topic_ID)
);

-- Table: User
CREATE TABLE User (
    User_ID int NOT NULL,
    user_name varchar(100) NOT NULL,
    password varchar(200) NOT NULL,
    UNIQUE INDEX User_ak_1 (user_name,password),
    CONSTRAINT User_pk PRIMARY KEY (User_ID)
);

-- foreign keys
-- Reference: Comment_Name_Lib (table: Comment)
ALTER TABLE Comment ADD CONSTRAINT Comment_Name_Lib FOREIGN KEY Comment_Name_Lib (random_name)
    REFERENCES Name_Lib (name_ID);

-- Reference: Comment_Post (table: Comment)
ALTER TABLE Comment ADD CONSTRAINT Comment_Post FOREIGN KEY Comment_Post (Post)
    REFERENCES Post (Post_ID);

-- Reference: Comment_User (table: Comment)
ALTER TABLE Comment ADD CONSTRAINT Comment_User FOREIGN KEY Comment_User (author)
    REFERENCES User (User_ID);

-- Reference: Name_Lib_Topic (table: Name_Lib)
ALTER TABLE Name_Lib ADD CONSTRAINT Name_Lib_Topic FOREIGN KEY Name_Lib_Topic (topic)
    REFERENCES Topic (Topic_ID);

-- Reference: Post_Category (table: Post)
ALTER TABLE Post ADD CONSTRAINT Post_Category FOREIGN KEY Post_Category (category)
    REFERENCES Category (Category_ID);

-- Reference: Post_Name_Lib (table: Post)
ALTER TABLE Post ADD CONSTRAINT Post_Name_Lib FOREIGN KEY Post_Name_Lib (random_name)
    REFERENCES Name_Lib (name_ID);

-- Reference: Post_User (table: Post)
ALTER TABLE Post ADD CONSTRAINT Post_User FOREIGN KEY Post_User (author)
    REFERENCES User (User_ID);

-- End of file.


```
