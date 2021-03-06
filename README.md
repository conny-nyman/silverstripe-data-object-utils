# silverstripe-data-object-utils

[![Build Status](https://travis-ci.com/conny-nyman/silverstripe-data-object-utils.png?branch=master)](https://travis-ci.org/conny-nyman/silverstripe-data-object-utils)
[![codecov](https://codecov.io/gh/conny-nyman/silverstripe-data-object-utils/branch/master/graph/badge.svg)](https://codecov.io/gh/conny-nyman/silverstripe-data-object-utils)

## Description

Useful constants, CMS field helper functions and a permission extension for [Silverstripe CMS ^4](https://github.com/silverstripe/silverstripe-cms).

Keep your SilverStripe code DRY!

## Constants

- Data types
- Search filter modifiers
- Data object fields
- Site tree fields
- Group fields

## CMS fields util functions

- Translate title and description on form fields using the same naming convention

## Test/Validation utils

- Generate error message strings

## Data object permission extension

Apply and configure:

- `canView`
- `canEdit`
- `canCreate`
- `canDelete`

permission checks to data objects through an extension.

## Example usages

### Apply permission checks to data objects

First set the allowed group codes in a config file.

```
Conan\DataObjectUtils\DataObjectGroupPermissionExtension:
  can_view_group_codes:
    - 'administrators'
    - 'can_view_group_codes'
  can_edit_group_codes:
    - 'administrators'
    - 'can_edit_group_codes'
  can_create_group_codes:
    - 'administrators'
    - 'can_create_group_codes'
  can_delete_group_codes:
    - 'administrators'
    - 'can_delete_group_codes'
```

Then apply the extension to data objects.

```
namespace The\Namespace;

use Conan\DataObjectUtils\DataObjectGroupPermissionExtension;
use SilverStripe\ORM\DataObject;

class MyClass extends DataObject 
{
    private static $extensions = [
        DataObjectGroupPermissionExtension::class,
    ];
}
```

### Translate CMS fields 

```
namespace The\Namespace;

use Conan\DataObjectUtils\DBConstants;
use Conan\DataObjectUtils\CMSFieldUtils;
use SilverStripe\ORM\DataObject;

class MyClass extends DataObject 
{
    const MY_STRING_FIELD = 'MyStringField';
    const MY_INT_FIELD = 'MyIntField';

    private static $db = [
        self::MY_STRING_FIELD => DBConstants::VARCHAR,    
        self::MY_INT_FIELD => DBConstants::INT,    
    ];

    public function getCMSFields() 
    {
        $fields = parent::getCMSFields();
        $myStringField = $fields->dataFieldByName(self::MY_STRING_FIELD);
        CMSFieldUtils::setTitle($myStringField, 'My title');
        CMSFieldUtils::setDescription($myStringField, 'My description');
    
        // Or for both title and description you can use this
        // CMSFieldUtils::setTitleAndDescription($myStringField, 'My title', 'My description');
    }
}
```

The field can then be translated like this:

```
en:
  The\Namespace\MyClass:
    MyStringField.Title: 'Translated title'
    MyStringField.Description: 'Translated description'
```

By default the namespaced class name is used, but it is possible to pass a custom value as the caller class.
e.g. `CMSFieldUtils::setTitle($myStringField, 'My title', 'MyModule');` and then translate it like this:

```
en:
  MyModule:
    MyStringField.Title: 'Translated title'
    MyStringField.Description: 'Translated description'
```

## Versioning

This library uses semantic versioning.

## License

`silverstripe-data-object-utils` is released under the MIT license. Please see the LICENSE file for more information.
