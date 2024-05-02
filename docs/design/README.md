# Проєктування бази даних

В рамках проекту розроблено: 
- модель бізнес-об'єктів 
- ER-модель
- реляційна схема

## Модель бізнес-об'єктів

@startuml
skinparam defaultFontName Courier New


entity User <<ENTITY>> #245280
entity User.password <<TEXT>> #8cb5de
entity User.mail <<TEXT>> #8cb5de
entity User.login <<TEXT>> #8cb5de
entity Role <<ENTITY>> #245280
entity User.name <<TEXT>> #8cb5de
entity User.id <<NUMBER>> #8cb5de

User.password -d-* User
User.mail -d-* User
User.login -r-* User
Role "1.1"--"0.*" User
User.name -d-* User
User.id -d-* User

entity Role.name <<TEXT>> #8cb5de
entity Role.id <<NUMBER>> #8cb5de

Role.name -d-* Role
Role.id -d-* Role

entity Permission_has_Role <<ENTITY>> #245280

Permission_has_Role "0.*"-d-"1.1" Role
entity Permission <<ENTITY>> #245280
entity Permission.id <<NUMBER>> #8cb5de
entity Permission.name <<TEXT>> #8cb5de
Permission "1.1"-r-"0.0" Permission_has_Role
Permission.id -d-* Permission
Permission.name -d-* Permission

entity Organization <<ENTITY>> #245280
entity Organization.id <<NUMBER>> #8cb5de
entity Organization.name <<TEXT>> #8cb5de
entity Organization.description <<TEXT>> #8cb5de

Organization.id -l-* Organization
Organization.name -d-* Organization
Organization.description -d-* Organization

entity Organization_list_has_Organizations <<ENTITY>> #245280

Organization "1.1"-d-"0.*" Organization_list_has_Organizations
entity Organization_list <<ENTITY>> #245280
entity Organization_list.id <<NUMBER>> #8cb5de
entity Organization_list.list_of_organizations <<TEXT>> #8cb5de
Organization_list_has_Organizations "0.*"-d-"1.1" Organization_list
Organization_list.id -d-* Organization_list
Organization_list.list_of_organizations -l-* Organization_list
Organization_list "1.1"-l-"0.*" User
entity Access <<ENTITY>> #245280
entity Access.id <<NUMBER>> #8cb5de
entity Access.time <<DATETIME>> #8cb5de
User "1.1"-d-"0.*" Access
Access.id -r-* Access
Access.time -l-* Access

entity Post <<ENTITY>> #245280
entity Post.id <<NUMBER>> #8cb5de
entity Post.name <<TEXT>> #8cb5de
entity Post.title <<TEXT>> #8cb5de
entity Post.description <<TEXT>> #8cb5de
entity Post.uploadedAt <<DATETIME>> #8cb5de
entity Post.updatedAt <<DATETIME>> #8cb5de

Access "0.*"-d-"1.1" Post
Post.id -u-* Post
Post.name -u-* Post
Post.title -u-* Post
Post.description -u-* Post
Post.uploadedAt -u-* Post
Post.updatedAt -u-* Post

entity Data <<ENTITY>> #245280
entity Data.id <<NUMBER>> #8cb5de
entity Data.name <<TEXT>> #8cb5de
entity Data.format <<TEXT>> #8cb5de
entity Data.size <<TEXT>> #8cb5de
entity Data.uploadedAt <<DATETIME>> #8cb5de

Data "0.*"-l-"1.1" Post
Data.id -d-* Data
Data.name -u-* Data
Data.format -l-* Data
Data.size -d-* Data
Data.uploadedAt -u-* Data

entity Category <<ENTITY>> #245280
entity Category.id <<NUMBER>> #8cb5de
entity Category.name <<TEXT>> #8cb5de
entity Category.description <<TEXT>> #8cb5de

Category "0.1"--"0.*" Category
Category "0.*"-r-"1.1" Post
Category.name -u-* Category
Category.id -u-* Category
Category.description -r-* Category

entity Rating <<ENTITY>> #245280
entity Rating.id <<NUMBER>> #8cb5de
entity Rating.value <<NUMBER>> #8cb5de

Rating "1.1"-r-"0.*" Post
Rating.value -u-* Rating
Rating.id -d-* Rating

@enduml



## ER-модель

@startuml


' Set some skin parameters to customize the appearance
skinparam arrowColor #666666
skinparam backgroundColor #F2F2F2
skinparam arrowheadWidth 2.0
skinparam arrowheadHeight 2.0

entity "User" <<ENTITY>>  {
  + id <<INT>>
  + login <<TEXT>> 
  + password <<TEXT>> 
  + mail <<TEXT>> 
  + name <<TEXT>>  
  + roleID <<INT>>
  + organizations_listID <<INT>> 
}

entity "Role" <<ENTITY>>  {
  + id <<INT>> 
  + name <<TEXT>>
  + description <<Text>>
}

entity "Permission_has_Role" {
  + permissionId <<INT>>
  + roleId  <<INT>>
}

entity "Permission" <<ENTITY>> {
  + id <<INT>>
  + name <<TEXT>>
}

entity Organization_list <<ENTITY>> {
  + id <<INT>>
  + list_of_organizations <<TEXT>>
}

entity "Organization_list_has_Organizations" <<ENTITY>> {
  + organization_listID <<INT>>
  + organizationsId <<INT>>
}

entity "Organizations" <<ENTITY>>  {
  + id <<INT>> 
  + name <<TEXT>> 
  + description <<TEXT>> 
}

entity "Access" <<ENTITY>>  {
  + id <<INT>> 
  + time <<DATETIME>> 
  + UserID <<INT>>
  + PostID <<INT>>
}
entity "Post" <<ENTITY>>  {
  + id <<INT>> 
  + name <<TEXT>> 
  + title <<TEXT>> 
  + description <<TEXT>> 
  + uploadedAt <<DATETIME>> 
  + updatedAt <<DATETIME>> 
  + RatingID <<INT>>
  + DataID <<INT>>
}

entity "Data" <<ENTITY>>  {
  + id <<INT>> 
  + name <<TEXT>> 
  + format <<TEXT>> 
  + size <<TEXT>> 
  + uploadedAt <<DATETIME>> 
}


entity "Category" <<ENTITY>>  {
  + id <<INT>> 
  + name <<TEXT>> 
  + description <<TEXT>> 
  + PostID <<INT>>
  + CategoryID <<INT>>
  + Category_PostID <<INT>>
}

entity "Rating" <<ENTITY>>  {
  + id <<INT>> 
  + value <<FLOAT>> 
}


Role "1.1" --> "0.*" User
Permission_has_Role "0.*" --> "1.1" Role
Permission "1.1" --> "0.*" Permission_has_Role
Organizations "1.1" --> "0.*" Organization_list_has_Organizations
Organization_list_has_Organizations "0.*" --> "1.1" Organization_list
Organization_list "1.1" --> "0.*" User
User "1.1" --> "0.*" Access
Access "0.*" --> "1.1" Post
Category "1.1" --> "0.*" Post
Category "0.1" --> "0.*" Category
Rating "1.1" --> "0.*" Post
Data "0.*" --> "1.1" Post
@enduml

## Реляційна схема

![relation_scheme](./photo/relation_scheme.png)