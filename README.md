# WP DB Helpers

**WP DB Helpers** is a WordPress-oriented PHP package that provides a set of utilities and a structured approach for interacting with the WordPress database using the `$wpdb` global object. The package is designed to simplify database operations and ensure consistency across your WordPress plugins or themes.

## Features

- **Abstract Repository:** Base class for implementing the repository pattern with `$wpdb`.
- **Pagination Support:** Easily paginate database results.
- **Bulk Operations:** Perform bulk inserts, updates, and deletes.
- **Transaction Management:** Start, commit, and rollback database transactions.
- **Error Logging:** Automatically log and debug database errors.
- **Dynamic Query Building:** Flexible support for building dynamic `WHERE` and `JOIN` clauses.

## Installation

This package can be included in plugin/theme or in bedrock/wordplate installation using Composer. 

```bash
composer require mzeahmed/wp-db-helpers
```

The package is not on Packagist, add it manually to your `composer.json`:

```json
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/mzeahmed/wp-db-helpers"
        }
    ],
    "require": {
        "mzeahmed/wp-db-helpers": "^1.0"
    }
}
```

## Usage

### 1. Implement a Repository

Create a repository class that extends the `AbstractRepository` class. For example:

```php
namespace YourNamespace\Repositories;

use Mzeahmed\WpDbHelpers\AbstractRepository;

class YourCustomRepository extends AbstractRepository
{
    public function __construct()
    {
        parent::__construct();
        $this->tableName = $this->wpdbPrefix . 'your_table_name';
    }

    public function findActiveItems(): array
    {
        return $this->findByCriteria(['status' => 'active']);
    }
}
```

### 2. Use the Repository

In your WordPress plugin or theme:

```php
use YourNamespace\Repositories\YourCustomRepository;

$repository = new YourCustomRepository();
$activeItems = $repository->findActiveItems();
```

### 3. Bulk Operations

Perform bulk inserts, updates, or deletes:

```php
// Bulk insert example
$repository->bulkInsert([
    ['column1' => 'value1', 'column2' => 'value2'],
    ['column1' => 'value3', 'column2' => 'value4'],
]);

// Bulk delete example
$repository->bulkDelete('id', [1, 2, 3]);
```

### 4. Transactions

Manage transactions to ensure data consistency:

```php
$repository->beginTransaction();

try {
    $repository->insert(['column1' => 'value1']);
    $repository->update(['column2' => 'value2'], ['id' => 1]);
    $repository->commit();
} catch (\Exception $e) {
    $repository->rollback();
    error_log($e->getMessage());
}
```

## Requirements

- PHP 8.3 or higher
- WordPress 6.0 or higher
- Composer for dependency management

## License

This package is licensed under the MIT License.
