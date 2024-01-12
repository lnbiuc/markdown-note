### 用户表

```sql
create table user
(
    id        int auto_increment,
    username  char(16) not null,
    password  char(18) null,
    telephone char(11) null,
    email     char(30) null,
    constraint user_pk
        primary key (id)
);

create unique index user_id_uindex
    on user (id);

create unique index user_username_uindex
    on user (username);


```

