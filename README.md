# Readme Documents Config


---


### set config file

change config file __config.php to config.php

change config file wt-admin/__config.php to wt-admin/config.php

### Change permisstion folder

/image/* to 777
/system/cache/* to 777
/system/logs/* to 777
/system/download/* to 777
/system/upload/* to 777

### Document config db migrations

สามารถอ่าน Document ฉบับเต็มได้ที่ 

[http://docs.phinx.org/en/latest/migrations.html#creating-a-new-migration](http://docs.phinx.org/en/latest/migrations.html#creating-a-new-migration)


Use Command line

`composer install`


url สำหรับลง composer [https://getcomposer.org/](https://getcomposer.org/)




เมื่อลงเสร็จแล้ว จะสามารถใช้คำสั้งของ phinx ได้

ถ้าลองพิมพ์   

`php vendor/bin/phinx`

จะมีคำสั่ง command line แสดงขึ้นมา

```

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  create       Create a new migration
  help         Displays help for a command
  init         Initialize the application for Phinx
  list         Lists commands
  migrate      Migrate the database
  rollback     Rollback the last or to a specific migration
  status       Show migration status
  test         Verify the configuration file
 seed
  seed:create  Create a new database seeder
  seed:run     Run database seeders 

```


และคำสั่งถัดไปคือ migration db เริ่มต้น แต่ก่อนจะ run คำสั้งนี้ ขอให้เปลียน config db ที่ไฟล์ config.php ให้ถูกต้องกับเครื่องของเราก่อนและเมื่อ config ถูกต้องแล้วให้ run คำสั้งนี้

`php vendor/bin/phinx migrate`

ผลที่ได้

![](http://abstractcodify.com/download/2.png)

แค่นีเราก็สามารถ run website ได้แล้ว


---------



### เร่ิมตอนการสร้าง Migration ของตัวเอง


ในขั้นตอนการสร้าง migration สามารถใช้คำสั่ง

`php vendor/bin/phinx create <ตามด้วยชื่อของ migration>`

![](http://abstractcodify.com/download/3.png)

เมื่อ run คำสั้งเสร็จจะแสดงดังรูปด้านบน โดย ที่อยู่ที่เก็บไฟล์ migrations ของ database จะเก็บไว้ที่ db/migrations/...  .php 


โดยเราจะเข้าไปเขียนคำสั้ง sql ในไฟล์ดังกล่าว โดยเข้าไปเขียนใน function change()


> ตัวอย่างเช่น



```php
    public function change()
    {
        // create the table
        $table = $this->table('user_logins');
        $table->addColumn('user_id', 'integer')
              ->addColumn('created', 'datetime')
              ->create();
    }

```
**หรือ**

```php
    public function change()
    {
        // create the table
        $this->execute("CREATE TABLE `weg_address` (
          `address_id` int(11) NOT NULL,
          `customer_id` int(11) NOT NULL,
          `firstname` varchar(32) NOT NULL,
          `lastname` varchar(32) NOT NULL,
          `company` varchar(40) NOT NULL,
          `address_1` varchar(128) NOT NULL,
          `address_2` varchar(128) NOT NULL,
          `city` varchar(128) NOT NULL,
          `postcode` varchar(10) NOT NULL,
          `country_id` int(11) NOT NULL DEFAULT '0',
          `zone_id` int(11) NOT NULL DEFAULT '0',
          `custom_field` text NOT NULL
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8;");
    }

```

**หรือ**

```php
    public function change()
    {
        // create the table
        $this->execute(file_get_contents( __DIR__ . '/wegota.sql'));
    }

```


สามารถเข้าไปอ่านข้อมูลเพิ่มเติมได้ที่  [http://docs.phinx.org/en/latest/migrations.html#creating-a-new-migration](http://docs.phinx.org/en/latest/migrations.html#creating-a-new-migration)





และเมื่อเขียนเสร็จแล้วก็จะสั้ง run migration ด้วย 

`php vendor/bin/phinx migrate`


เพื่อให้ migration file ที่เราเขียนไว้ทำงาน  



---



ตัวนี้เมื่อสั่ง git pull ลงมา หรือ Download file ลงมา ให้ทำการ `php vendor/bin/phinx migrate` เพื่อเป็นการลงข้อมูลดาต้าเบสใหม่ในครั้งแรก เพราน้ามได้เอาตัว database เร่ิมต้นใส่ไว้ให้แล้ว เมื่อทำการ pull code นี้มาในครั้งแรก และติดตั้ง composer แล้วทำการสั้ง 

`php vendor/bin/phinx migrate` 

ระบบก็จะทำการลง database ให้เลย  









