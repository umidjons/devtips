# Unit testing model using dbunit phpunit extension

I assume, that you already have `phpunit` installed and added into `PATH`.
Create `guestbook` database in `mysql`.
Create `guestbook` table in that database.

## Create model to test
File `src\Guestbook.php`
```php
<?php

namespace app\models;

use \PDO;

const DB_DSN = 'mysql:dbname=guestbook;host=localhost';
const DB_USER = 'root';
const DB_PASSWD = '';

class Guestbook
{
    public $id;
    public $content;
    public $user;
    public $created;

    protected $db;

    public function __construct()
    {
        $options = [
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC
        ];

        $this->db = new PDO(DB_DSN, DB_USER, DB_PASSWD, $options);
    }

    /**
     * Adds current instance into database.
     * Populates id with newly inserted row id.
     */
    public function add()
    {
        $sql = "insert into guestbook set `content`=:pContent, `user`=:pUser, `created`=:pCreated";
        $sth = $this->db->prepare($sql);
        $sth->execute(
            [
                ':pContent' => $this->content,
                ':pUser' => $this->user,
                ':pCreated' => $this->created
            ]
        );
        $this->id = $this->db->lastInsertId();
    }
}
```

## Preparing fixture
File `tests/_data/guestbook.yml`:
```yaml
guestbook:
  -
    id: 1
    content: "Hello buddy!"
    user: "joe"
    created: "2016-12-11 09:30:50"
  -
    id: 2
    content: "I like it!"
    user: "jane"
    created: "2016-12-10 11:20:15"
```

## PHPUnit configuration file
I enabled output colors and verbosity in configuration.
Also created global variables to use in test classes to connect to the database.
File `tests/phpunit.xml`
```xml
<phpunit colors="true" verbose="true">
    <php>
        <includePath>src</includePath>

        <var name="DB_DSN" value="mysql:dbname=guestbook;host=localhost"/>
        <var name="DB_NAME" value="guestbook"/>
        <var name="DB_USER" value="root"/>
        <var name="DB_PASSWD" value=""/>
    </php>

    <testsuites>
        <testsuite name="GuestBook">
            <directory>tests</directory>
        </testsuite>
    </testsuites>
</phpunit>
```

## Test class
File `tests/GuestbookTest.php`
```php
<?php

// includePath configured in phpunit.xml, so following will work
include_once 'Guestbook.php';

// if no includePath configured, then use following
// include_once 'src/Guestbook.php';

use app\models\Guestbook;

class GuestbookTest extends PHPUnit_Extensions_Database_TestCase
{

    static private $pdo = null;

    private $conn = null;

    public function getConnection()
    {
        if (!$this->conn) {
            if (!self::$pdo) {
                self::$pdo = new PDO($GLOBALS['DB_DSN'], $GLOBALS['DB_USER'], $GLOBALS['DB_PASSWD']);
            }
            $this->conn = $this->createDefaultDBConnection(self::$pdo, $GLOBALS['DB_NAME']);
        }

        return $this->conn;
    }

    public function getDataSet()
    {
        // fill tables with data
        return new PHPUnit_Extensions_Database_DataSet_YamlDataSet(__DIR__ . '/_data/guestbook.yml');
    }

    public function testGuestbookHasTwoRows()
    {
        // there should be 2 rows in guestbook table
        $this->assertEquals(2, $this->getConnection()->getRowCount('guestbook'));
    }

    public function testGuestbookAddThirdRow()
    {
        // add new row into guestbook table
        $gb = new Guestbook();
        $gb->content = 'Third';
        $gb->user = 'john';
        $gb->created = '2016-12-11 10:00:00';
        $gb->add();

        // now there should be 3 rows in guestbook table
        $this->assertEquals(3, $this->getConnection()->getRowCount('guestbook'));

        // get last inserted rows - 3rd row
        $queryTable = $this->getConnection()->createQueryTable('guestbook', 'select * from guestbook');
        $lastRow = $queryTable->getRow(2); // get 3rd row

        // compare last row fields with guestbook object properties
        $this->assertEquals($gb->id, $lastRow['id']);
        $this->assertEquals($gb->content, $lastRow['content']);
        $this->assertEquals($gb->user, $lastRow['user']);
        $this->assertEquals($gb->created, $lastRow['created']);
    }

}
```

## Run tests
Change directory into project folder, then execute following commands:
```sh
phpunit
```

Sample output:
```
PHPUnit 5.7.2 by Sebastian Bergmann and contributors.

Runtime:       PHP 5.6.23 with Xdebug 2.4.0
Configuration: F:\OpenServer\domains\t2\phpunit.xml

..                                                                  2 / 2 (100%)

Time: 464 ms, Memory: 9.50MB

OK (2 tests, 6 assertions)
```
