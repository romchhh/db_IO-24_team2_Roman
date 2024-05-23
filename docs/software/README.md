# Реалізація інформаційного та програмного забезпечення

В рамках проекту розробляється: 
## SQL-скрипт для створення на початкового наповнення бази даних

```sql
    -- MySQL Workbench Forward Engineering
    
    SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
    SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
    SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';
    
    -- -----------------------------------------------------
    -- Schema mydb
    -- -----------------------------------------------------
    DROP SCHEMA IF EXISTS `mydb` ;
    
    -- -----------------------------------------------------
    -- Schema mydb
    -- -----------------------------------------------------
    CREATE SCHEMA IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 ;
    USE `mydb` ;
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Character`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Character` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Character` (
      `Character.id` INT NOT NULL,
      `Character.name` VARCHAR(45) NULL,
      PRIMARY KEY (`Character.id`))
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`User`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`User` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`User` (
      `User.id` INT NOT NULL,
      `User.username` VARCHAR(45) NULL,
      `User.email` VARCHAR(45) NULL,
      `User.password` VARCHAR(45) NULL,
      `User.firstname` VARCHAR(45) NULL,
      `User.lastname` VARCHAR(45) NULL,
      `Usercol` VARCHAR(45) NULL,
      `Character_Character.id` INT NOT NULL,
      PRIMARY KEY (`User.id`),
      INDEX `fk_User_Character1_idx` (`Character_Character.id` ASC) VISIBLE,
      CONSTRAINT `fk_User_Character1`
        FOREIGN KEY (`Character_Character.id`)
        REFERENCES `mydb`.`Character` (`Character.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Datafile`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Datafile` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Datafile` (
      `Datafile.id` INT NOT NULL,
      `Datafile.name` VARCHAR(45) NULL,
      `Datafile.content` VARCHAR(45) NULL,
      `Datafile.description` VARCHAR(45) NULL,
      `Datafile.format` VARCHAR(45) NULL,
      `Datafile.date` DATETIME NULL,
      PRIMARY KEY (`Datafile.id`))
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Access`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Access` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Access` (
      `Access.id` INT NOT NULL,
      `User_User.id` INT NOT NULL,
      `Datafile_Datafile.id` INT NOT NULL,
      PRIMARY KEY (`Access.id`),
      INDEX `fk_Access_User1_idx` (`User_User.id` ASC) VISIBLE,
      INDEX `fk_Access_Datafile1_idx` (`Datafile_Datafile.id` ASC) VISIBLE,
      CONSTRAINT `fk_Access_User1`
        FOREIGN KEY (`User_User.id`)
        REFERENCES `mydb`.`User` (`User.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_Access_Datafile1`
        FOREIGN KEY (`Datafile_Datafile.id`)
        REFERENCES `mydb`.`Datafile` (`Datafile.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Request`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Request` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Request` (
      `Request.id` INT NOT NULL,
      `Request.type` VARCHAR(45) NULL,
      `Request.message` VARCHAR(45) NULL,
      `User_User.id` INT NOT NULL,
      `Access_Access.id` INT NOT NULL,
      PRIMARY KEY (`Request.id`, `User_User.id`, `Access_Access.id`),
      INDEX `fk_Request_User1_idx` (`User_User.id` ASC) VISIBLE,
      INDEX `fk_Request_Access1_idx` (`Access_Access.id` ASC) VISIBLE,
      CONSTRAINT `fk_Request_User1`
        FOREIGN KEY (`User_User.id`)
        REFERENCES `mydb`.`User` (`User.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_Request_Access1`
        FOREIGN KEY (`Access_Access.id`)
        REFERENCES `mydb`.`Access` (`Access.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Mark`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Mark` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Mark` (
      `Mark.name` VARCHAR(45) NOT NULL,
      PRIMARY KEY (`Mark.name`))
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Datafile_mark`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Datafile_mark` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Datafile_mark` (
      `Datafile_Datafile.id` INT NOT NULL,
      `Mark_Mark.name` VARCHAR(45) NOT NULL,
      PRIMARY KEY (`Datafile_Datafile.id`, `Mark_Mark.name`),
      INDEX `fk_Datafile_Mark_Mark1_idx` (`Mark_Mark.name` ASC) VISIBLE,
      CONSTRAINT `fk_Datafile_Mark_Datafile1`
        FOREIGN KEY (`Datafile_Datafile.id`)
        REFERENCES `mydb`.`Datafile` (`Datafile.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_Datafile_mark_Mark1`
        FOREIGN KEY (`Mark_Mark.name`)
        REFERENCES `mydb`.`Mark` (`Mark.name`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Permission`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Permission` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Permission` (
      `Permission.id` INT NOT NULL,
      `Permission.name` VARCHAR(45) NULL,
      PRIMARY KEY (`Permission.id`))
    ENGINE = InnoDB;
    
    
    -- -----------------------------------------------------
    -- Table `mydb`.`Provide_ Permission`
    -- -----------------------------------------------------
    DROP TABLE IF EXISTS `mydb`.`Provide_ Permission` ;
    
    CREATE TABLE IF NOT EXISTS `mydb`.`Provide_ Permission` (
      `Permission_Permission.id` INT NOT NULL,
      `Character_Character.id` INT NOT NULL,
      PRIMARY KEY (`Permission_Permission.id`, `Character_Character.id`),
      INDEX `fk_Provide_ Permission_Character1_idx` (`Character_Character.id` ASC) VISIBLE,
      CONSTRAINT `fk_Provide_ Permission_Permission`
        FOREIGN KEY (`Permission_Permission.id`)
        REFERENCES `mydb`.`Permission` (`Permission.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION,
      CONSTRAINT `fk_Provide_ Permission_Character1`
        FOREIGN KEY (`Character_Character.id`)
        REFERENCES `mydb`.`Character` (`Character.id`)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION)
    ENGINE = InnoDB;
    
    
    SET SQL_MODE=@OLD_SQL_MODE;
    SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
    SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
    
    -- -----------------------------------------------------
    -- Data for table `mydb`.`Character`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Character` (`Character.id`, `Character.name`) VALUES (1, 'sysadmin');
    INSERT INTO `mydb`.`Character` (`Character.id`, `Character.name`) VALUES (2, 'developer');
    INSERT INTO `mydb`.`Character` (`Character.id`, `Character.name`) VALUES (3, 'moderator');
    INSERT INTO `mydb`.`Character` (`Character.id`, `Character.name`) VALUES (4, 'user');
    
    COMMIT;
    
    
    -- -----------------------------------------------------
    -- Data for table `mydb`.`User`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`User` (`User.id`, `User.username`, `User.email`, `User.password`, `User.firstname`, `User.lastname`, `Usercol`, `Character_Character.id`) VALUES (1, 'user1', 'user1@gmal.com', 'abcdfg', 'Guy', 'Ritchie', 'a', 1);
    INSERT INTO `mydb`.`User` (`User.id`, `User.username`, `User.email`, `User.password`, `User.firstname`, `User.lastname`, `Usercol`, `Character_Character.id`) VALUES (2, 'user2', 'user2@gmal.com', 'abcdfo', 'Big', 'Lebovski', 'b', 3);
    INSERT INTO `mydb`.`User` (`User.id`, `User.username`, `User.email`, `User.password`, `User.firstname`, `User.lastname`, `Usercol`, `Character_Character.id`) VALUES (3, 'user3', 'user3@gmal.com', 'abcdfp', 'Quentin', 'Tarantino', 'c', 4);
    
    COMMIT;
    
    
    -- -----------------------------------------------------
    -- Data for table `mydb`.`Datafile`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Datafile` (`Datafile.id`, `Datafile.name`, `Datafile.content`, `Datafile.description`, `Datafile.format`, `Datafile.date`) VALUES (1, 'Schema', 'photo', 'schema of relations', 'png', '2024-5-12');
    INSERT INTO `mydb`.`Datafile` (`Datafile.id`, `Datafile.name`, `Datafile.content`, `Datafile.description`, `Datafile.format`, `Datafile.date`) VALUES (2, 'Docs', 'information', 'information about team', 'word', '2024-5-13');
    INSERT INTO `mydb`.`Datafile` (`Datafile.id`, `Datafile.name`, `Datafile.content`, `Datafile.description`, `Datafile.format`, `Datafile.date`) VALUES (3, 'Docs', 'information', 'development information', 'word', '2024-5-14');
    
    COMMIT;
    

    -- -----------------------------------------------------
    -- Data for table `mydb`.`Access`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Access` (`Access.id`, `User_User.id`, `Datafile_Datafile.id`) VALUES (1, 1, 1);
    INSERT INTO `mydb`.`Access` (`Access.id`, `User_User.id`, `Datafile_Datafile.id`) VALUES (2, 2, 2);
    INSERT INTO `mydb`.`Access` (`Access.id`, `User_User.id`, `Datafile_Datafile.id`) VALUES (3, 3, 3);

    COMMIT;
    

    -- -----------------------------------------------------
    -- Data for table `mydb`.`Request`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Request` (`Request.id`, `Request.type`, `Request.message`, `User_User.id`, `Access_Access.id`) VALUES (1, 'login', 'You are logined', 1, 1);
    INSERT INTO `mydb`.`Request` (`Request.id`, `Request.type`, `Request.message`, `User_User.id`,     `Access_Access.id`) VALUES (2, 'login', 'You are logined', 2, 2);
    INSERT INTO `mydb`.`Request` (`Request.id`, `Request.type`, `Request.message`, `User_User.id`,     `Access_Access.id`) VALUES (3, 'registration', 'You are registered', 3, 3);
    
    COMMIT;
    
    
    -- -----------------------------------------------------
    -- Data for table `mydb`.`Mark`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Mark` (`Mark.name`) VALUES ('abc');
    INSERT INTO `mydb`.`Mark` (`Mark.name`) VALUES ('acb');
    INSERT INTO `mydb`.`Mark` (`Mark.name`) VALUES ('bac');
    
    COMMIT;
    
    
    -- -----------------------------------------------------
    -- Data for table `mydb`.`Datafile_mark`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Datafile_mark` (`Datafile_Datafile.id`, `Mark_Mark.name`) VALUES (1, 'abc');
    INSERT INTO `mydb`.`Datafile_mark` (`Datafile_Datafile.id`, `Mark_Mark.name`) VALUES (2, 'acb');
    INSERT INTO `mydb`.`Datafile_mark` (`Datafile_Datafile.id`, `Mark_Mark.name`) VALUES (3, 'bac');
    
    COMMIT;
    
    
    -- -----------------------------------------------------
    -- Data for table `mydb`.`Permission`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Permission` (`Permission.id`, `Permission.name`) VALUES (1, 'Create');
    INSERT INTO `mydb`.`Permission` (`Permission.id`, `Permission.name`) VALUES (2, 'Edit');
    INSERT INTO `mydb`.`Permission` (`Permission.id`, `Permission.name`) VALUES (3, 'Delete');
    INSERT INTO `mydb`.`Permission` (`Permission.id`, `Permission.name`) VALUES (4, 'Execute');
    
    COMMIT;
    
    
    -- -----------------------------------------------------
    -- Data for table `mydb`.`Provide_ Permission`
    -- -----------------------------------------------------
    START TRANSACTION;
    USE `mydb`;
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (1, 1);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (1, 2);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (1, 3);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (2, 1);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (2, 2);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (2, 3);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (2, 4);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (3, 1);
    INSERT INTO `mydb`.`Provide_ Permission` (`Permission_Permission.id`, `Character_Character.id`) VALUES (3, 3);

    COMMIT;
```
## RESTfull сервіс для управління даними

- app.py:
```python
from app_init import app
import users_controller

if __name__ == "__main__":
    app.run(debug=True, port=5000, host="127.0.0.1")
```

- app_init.py:
```python
from flask import Flask

app = Flask(__name__)
```

- user_module.py:
```python

import mysql.connector


class Users:
    def init(self):
        try:
            self.host = 'localhost'
            self.user = 'root'
            self.password = 'io24_2'
            self.db = 'mydb'

            self.linking = mysql.connector.connect(host=self.host,
                                                      user=self.user,
                                                      password=self.password,
                                                      database=self.db)

            self.tag = self.linking.tag(dictionary=True)
            print("Successful connection to database")
        except mysql.connector.Error as err:
            print("Failed to connect to database:", err)

    def get_all_users(self):
        try:
            self.tag.execute("select * from user")
            users = self.tag.fetchall()

            if self.tag.rowcount == 0:
                return {"message": "No users", "error": "Not Found", "status_code": 404}

            return users
        except mysql.connector.Error as err:
            return {'message': 'Failed to get all users', 'error': str(err), 'status_code': 500}

    def get_user_by_id(self, user_id):
        try:
            user_id = int(user_id)
            self.tag.execute("select * from user where User.id = %s", (user_id,))
            user = self.tag.fetchone()

            if self.tag.rowcount == 0:
                return {"message": f"No user with id {user_id}", "error": "Not Found", "status_code": 404}

            return user
        except mysql.connector.Error as err:
            return {'message': 'Failed to get user', 'error': str(err), 'status_code': 500}
        except ValueError:
            return {"message": "Invalid user id", "error": "Bad Request", "status_code": 400}

    def add_user(self, info):
        try:
            self.tag.execute('start transaction')
            self.tag.execute(f"insert into user (User.id, User.username, User.email, User.password, "
                                f"User.firstname, User.lastname, Usercol, Role_Role.id) "
                                f"values {tuple([i for i in info.values()])}")
            self.linking.commit()

            if self.tag.rowcount > 0:
                return {"message": "User added to database", "status_code": 200}
            else:
                return {"message": "User was not added to database", "error": "Not Acceptable", "status_code": 406}
        except mysql.connector.Error as err:
            self.linking.rollback()
            return {'message': 'Failed to add user', 'error': str(err), 'status_code': 500}


    def delete_user(self, user_id):
        try:
            user_id = int(user_id)
            self.tag.execute('start transaction')
            rows_deleted = 0
            self.tag.execute("delete from user where User.id = %s", (user_id,))
            rows_deleted += self.tag.rowcount
            self.tag.execute("delete from request where User_User.id = %s", (user_id,))
            rows_deleted += self.tag.rowcount
            self.tag.execute("delete from access where User_User.id = %s", (user_id,))
            rows_deleted += self.tag.rowcount
            self.linking.commit()
            if rows_deleted > 0:
                return {"message": f"User {user_id} deleted from database", "status_code": 204}
            else:
                return {"message": f"User {user_id} was not deleted from database",
                        "error": "Not Found", "status_code": 404}
        except mysql.connector.Error as err:
            self.linking.rollback()
            return {'message': 'Failed to delete user', 'error': str(err), 'status_code': 500}
        except ValueError:
            return {"message": "Invalid user id", "error": "Bad Request", "status_code": 400}
            def update_user(self, user_id, info):
        try:
            user_id = int(user_id)
            self.tag.execute('start transaction')
            updated_rows = 0
            for i in info.items():
                self.tag.execute(f"update user set {i[0]} = '{i[1]}' where User.id = {user_id}")
                updated_rows += 1
            self.linking.commit()

            if updated_rows > 0:
                return {"message": f"User {user_id} updated in database", "status_code": 200}
            else:
                return {"message": f"User {user_id} was not updated in database",
                        "error": "Not Acceptable", "status_code": 406}
        except mysql.connector.Error as err:
            self.linking.rollback()
            return {'message': 'Failed to update user', 'error': str(err), 'status_code': 500}
        except ValueError:
            return {"message": "Invalid user id", "error": "Bad Request", "status_code": 400}
```

- user_controller.py:
```python
from flask import request, jsonify
from user_model import Users
from app_init import app

users = Users()


@app.route("/users", methods=['GET'])
def get_all_users():
    return jsonify(users.get_all_users())


@app.route("/user/<user_id>", methods=['GET'])
def get_user_by_id(user_id):
    return jsonify(users.get_user_by_id(user_id))


@app.route("/users/add", methods=['POST'])
def add_user():
    url_params = request.args.to_dict()
    return jsonify(users.add_user(url_params))


@app.route("/users/delete/<user_id>", methods=['DELETE'])
def delete_user(user_id):
    return jsonify(users.delete_user(user_id))


@app.route("/users/update/<user_id>", methods=['PUT'])
def update_user(user_id):
    url_params = request.args.to_dict()
    return jsonify(users.update_user(user_id, url_params))
```