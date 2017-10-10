# Integration testing Yii2 model with Codeception using fixtures and Yii2 module

## Prepare database

Create database for tests.
```mysql
CREATE DATABASE mydb_test CHARSET=utf8 COLLATE=utf8_unicode_ci;
```

Create tables in test database.
```mysql
CREATE TABLE `books` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `author` varchar(50) COLLATE utf8_unicode_ci NOT NULL,
  `published` date NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
```

Set test database name in `config/test_db.php`:
```php
<?php
$db = require(__DIR__ . '/db.php');
$db['dsn'] = 'mysql:host=localhost;dbname=mydb_test';
return $db;
```

## Book model

File `models/Book.php`:
```php
<?php

namespace app\models;

use Yii;

/**
 * This is the model class for table "books".
 *
 * @property integer $id
 * @property string $title
 * @property string $author
 * @property string $published
 */
class Book extends \yii\db\ActiveRecord
{

    /**
     * @inheritdoc
     */
    public static function tableName()
    {
        return 'books';
    }

    /**
     * @inheritdoc
     */
    public function rules()
    {
        return [
            [['title', 'author', 'published',], 'required'],
            [['published'], 'safe'],
            [['title'], 'string', 'max' => 255],
            [['author'], 'string', 'max' => 50],
        ];
    }

    /**
     * @inheritdoc
     */
    public function attributeLabels()
    {
        return [
            'id' => 'ID',
            'title' => 'Title',
            'author' => 'Author',
            'published' => 'Published Date',
        ];
    }
}
```

## Prepare fixture

Enable `Yii2` module in `tests/unit.suite.yml`:
```yaml
class_name: UnitTester
modules:
    enabled:
      - Asserts
      - Yii2
```

Create `tests/fixtures/BookFixture.php`:
```php
<?php

namespace app\tests\fixtures;

use yii\test\ActiveFixture;

class BookFixture extends ActiveFixture
{
    public $modelClass = 'app\models\Book';
}
```

Create fixture data in `tests/fixtures/data/books.php`:
```php
<?php

return [
    'book1' => [
        'id' => 1,
        'title' => 'First Book',
        'author' => 'John Doe',
        'published' => '2016-11-23'
    ],
    'book2' => [
        'id' => 2,
        'title' => 'Second Book',
        'author' => 'Jane Doe',
        'published' => '2015-10-17'
    ],
];
```

## Test implementation

Implement integration tests for Book in `tests/unit/models/BookTest.php`:
```php
<?php
namespace tests\models;

use app\models\Book;
use app\tests\fixtures\BookFixture;

class BookTest extends \Codeception\Test\Unit
{
    /**
     * @var \UnitTester
     */
    public $tester;

    public function _fixtures()
    {
        return ['books' => BookFixture::className()];
    }

    public function testAddingNewBookWithCorrectDataWillSuccess()
    {
        $this->assertCount(2, Book::find()->all());
        $this->tester->seeRecord(Book::className(), ['title' => 'First Book']);
        $this->tester->seeRecord(Book::className(), ['title' => 'Second Book']);

        $book = new Book();
        $book->title = 'Third Book';
        $book->author = 'Mike';
        $book->published = '2016-12-31';

        $book->save();

        $this->assertNotNull($book->id);

        $this->assertCount(3, Book::find()->all());
        $this->tester->seeRecord(Book::className(), ['id' => 3, 'title' => 'Third Book']);
    }
}
```

## Test execution & output

Run test:
```sh
composer exec codecept run unit -- -v models/BookTest.php
```

Sample output:
```
$ composer exec codecept run unit -- -v models/BookTest.php
> __exec_command: codecept "run" "unit" "-v" "models/BookTest.php"
Codeception PHP Testing Framework v2.2.7
Powered by PHPUnit 5.7.2 by Sebastian Bergmann and contributors.


Unit Tests (1) -------------------------------------------------------------------------------------------------------------------------------------------------

Modules: Asserts, Yii2
----------------------------------------------------------------------------------------------------------------------------------------------------------------

- BookTest: Adding new book with correct data will success
+ BookTest: Adding new book with correct data will success (0.07s)
----------------------------------------------------------------------------------------------------------------------------------------------------------------


Time: 1.36 seconds, Memory: 11.50MB

OK (1 test, 3 assertions)

```
