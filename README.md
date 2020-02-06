# Laravel Rot Ebal Nova Patches
A set of patches that fix bugs and improve the functionality of Laravel Nova.

Currently developed, compatible and tested with Nova 2.9.3.


## Installation

You can install the package in to a Laravel app that uses Nova via composer:

```bash
composer require laravelrotebal/nova-patches
```

All patches will be applied automatically!


## Why is this package needed?

- [x] Core bug fixes.
- [x] Core improved functioning.
- [x] Core smell code correction.
- [x] More flexible core code for best practices.


## TODO

- [ ] Manually choose which patches to apply.
- [ ] Automatically rebuild and publish frontend resources.
- [ ] Or find a way to make patches for frontend compiled resources.
- [ ] Create tests for testing patched functionality.
- [ ] Compatible with other versions of Nova.
- [ ] Uniform naming of patches.


## Depends on the package vaimo/composer-patches

Applies a patch from a local or remote file to any package that is part of a given composer project. Patches can be defined both on project and on package level. Optional support for patch versioning, sequencing, custom patch applier configuration and composer command for testing/troubleshooting patches.


## How it works

Directory `patches/laravel/nova` contains patch files. One patch file for one bugfix or feature improvement.

Section `$.extra.patches.laravel/nova` in `composer.json` contains the patch files to be applied in the specified order.

After run `install`, `require`, `update`, `patch:redo`, `patch:undo` commands of `composer` the specified patches will be applied to the specified packages in the `vendor` directory.

Some patches may modify the source files for the frontend. Which will require manual start of the assembly of resources and possibly their publication.


## Patch list


### refactoring-reformat-fields-js.patch

Change the format to simplify the applying of patches.

Affected files:

- resources/js/fields.js

Need rebuild frontend: no


### has-many-index-template.patch

HasManyField can be displayed in the index.

Affected files:

- resources/js/components/Index/HasManyField.vue
- resources/js/fields.js
- src/Fields/HasMany.php

Need rebuild frontend: yes


### has-one-index-template.patch

HasOneField can be displayed in the index.

Affected files:

- resources/js/components/Index/HasOneField.vue
- resources/js/fields.js
- src/Fields/HasOne.php

Need rebuild frontend: yes


### action-event-cli-compatible.patch

forAttachedResource(), forAttachedResourceUpdate() of ActionEvent independent of NovaRequest.

Affected files:

- src/Http/Controllers/AttachedResourceUpdateController.php
- src/Http/Controllers/PivotFieldDestroyController.php
- src/Http/Controllers/ResourceAttachController.php
- src/Actions/ActionEvent.php

Need rebuild frontend: no


### fix-resource-detach.patch

ActionEvent::forResourceDetach() can do mass detachment.

Affected files:

- src/Actions/ActionEvent.php
- src/Http/Controllers/ResourceDetachController.php

Need rebuild frontend: no


### action-event-create-without-duplicates.patch

ActionEvent::forResourceCreate() will not duplicate creation events.

Affected files:

- src/Actions/ActionEvent.php

Need rebuild frontend: no 


## How to rebuild frontend

Go to `laravel/nova` directory and execute:

```
npm i && cp webpack.mix.js.dist webpack.mix.js && npm run prod
```

For example, I have a rebuild process in 38 seconds. This is a long time!

After successful rebuild, be sure to run:

```
php artisan nova:publish
```
