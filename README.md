# Laravel integration for the Grid.js

[![Latest Version on Packagist](https://img.shields.io/packagist/v/wdev-rs/laravel-datagrid.svg?style=flat-square)](https://packagist.org/packages/wdev-rs/laravel-datagrid)
[![Build Status](https://github.com/wdev-rs/laravel-datagrid/actions/workflows/test.yml/badge.svg)](https://github.com/wdev-rs/laravel-datagrid/actions/workflows/test.yml)
[![Quality Score](https://img.shields.io/scrutinizer/g/wdev-rs/laravel-datagrid.svg?style=flat-square)](https://scrutinizer-ci.com/g/wdev-rs/laravel-datagrid)
[![Total Downloads](https://img.shields.io/packagist/dt/wdev-rs/laravel-datagrid.svg?style=flat-square)](https://packagist.org/packages/wdev-rs/laravel-datagrid)

This package is a Laravel integration for the [Grid.js](https://gridjs.io/). The packages makes it easy to create data-grid for your Laravel application, for example admin panel lists.

## Installation

You can install the package via composer:

```bash
composer require wdev-rs/laravel-datagrid
```

Install the Vue.js integration:

```bash
npm install gridjs-vue
```

Publish the vendor files by running

```bash
php artisan vendor:publish --provider="WdevRs\LaravelDatagrid\LaravelDatagridServiceProvider"
```

Register the DataGrid fronted Vue.js component by adding the following line to your `app.js`:

```javascript
require('./vendor/laravel-datagrid/laravel-datagrid');
```

## Usage
The base of this package is the `\WdevRs\LaravelDatagrid\DataGrid\DataGrid` class. This class is used to define the 
columns and the behavior of the datagrid. While you can use this class directly from the controller, I'll 
suggest extending it and create separate classes for each datagrid.

``` php
class CategoriesDataGrid extends DataGrid
{

    /**
     * CategoriesDataGrid constructor.
     */
    public function __construct()
    {
        $this->query(Category::query())
            ->column('id', 'ID', null, 50)
            ->column('name', 'Name', function ($category) {
                return view('admin.categories.actions.edit_link', ['category' => $category])->render();
            })
    }
}
```

Using the `query` method you can define what should be the base query for the DataGrid. It accepts a Laravel Query Builder object.
The `column` method is used to define the columns of the DataGrid, the argument are as follows:
- `id` - the name of the field in the database
- `name` - the label which should appear in the DataGrid column header
- `formatter` - optional, callable allows you to format the display of the column. As you can see from the above example
  probably the most elegant way to do this is to include a blade view and render it.
- `width` - optional, the with of the column

The frontend component of the DataGrid can be found in the `resources/js/vendor/laravel-datagrid/components/DataGrid.vue`
By default DataGrid comes with one row action, which is the delete action. This action can be found in the following file: 
`resources/js/vendor/laravel-datagrid/actions/delete.js`

You can extend it with more custom actions by creating them based on the existing one. To add the to the datagrid,
extend the `cols` definition in the `DataGrid.vue`:
```javascript
            cols: this.columns.map((col) => {col.formatter = (cell) => html(cell); return col;}).concat(
                [{
                    name: 'Actions',
                    sort: false,
                    width: 50,
                    formatter: (cell, row) => {
                        return h('div', {className: "text-center"},
                            deleteAction.call(this, row.cells[0].data,row.cells[1].data),
                            yourCustomAction.call(this, row.cells[0].data,row.cells[1].data)
                        )
                    }
                }]
            )
```

### Testing

``` bash
composer test
```

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email daniel@wdev.rs instead of using the issue tracker.

## Credits

- [Daniel Werner](https://github.com/wdev-rs)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

## Laravel Package Boilerplate

This package was generated using the [Laravel Package Boilerplate](https://laravelpackageboilerplate.com).
