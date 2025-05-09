## x.y.z

Miscellaneous:

- Add py.typed file

## 5.2.0

Feature:
- Allow ignoring all initial migrations, with `--ignore-initial-migrations`

Bug:
- Don't detect an index creation during a transaction with an exclusive lock, when the table is being created (#264)

Miscellaneous:

- Add support for Python 3.13
- Add support for Django 5.1
- Drop support for Python 3.7 and 3.8
- Mark md5 hashing as not used for security

## 5.1.0

Feature:
- Support Django 5.0 `db_default` attribute (issue #275)
- Allow ignoring the failures of `sqlmigrate` commands, with `--ignore-sqlmigrate-errors` option (issue #274)

Bug:
- Don't detect 'IS NOT NULL' as backward incompatible changes (issue #263)
- Don't consider UNIQUE INDEX creation as making a column not nullable

Miscellaneous:

- Migrated from `setup.py` and `setup.cfg` to `pyproject.toml`
- Add support for Python 3.12
- Add support for Django 5.0
- Avoid git command injections
- Remove Codecov integration

## 5.0.0

- **Breaking change**: stop silently ignoring when the internal `sqlmigrate` call fails and the linter cannot analyse the migration.
Instead, the linter crashes and lets the `sqlmigrate` error raise, in order to avoid letting a problematic migration pass.
One common reason for such an error is the SQL generation which requires the database to be actually migrated in order to fetch actual constraint names from it.
The crash is a sign to double-check the migration. But if you are certain the migration is safe, you can ignore it (issue #209)

Features:

- Fixed `RunPython` model import check when using a `through` object like `MyModel.many_to_many.through.objects.filter(...)` (issue #218)
- Mark the `IgnoreMigration` operation as `elidable=True`
- Handle `functools.partial` functions in RunPython data migrations
- Add a new check, `CREATE_INDEX_EXCLUSIVE` to detect index creation while an exclusive lock is held

Bug:

- Don't detect not nullable field on partial index creation (issue #250)

Miscellaneous:

- Add support for Python 3.11
- Add support for Django 4.1
- Add support for Django 4.2
- Drop support for Django 2.2
- Internally rename "migration tests" to "migration checks"
- Add dataclasses internally instead of custom dicts
- Use pre-commit hooks for linting
- Add `mypy` and `ruff` usages

## 4.1.0

- Allow configuring logging for `makemigrations` command and unify behaviour with `lintmigrations` (issue #207)
- Adapt `--warnings-as-errors` option to allow selecting some migration tests only (issue #201)
- Add `sql_analyser` option to `makemigrations` in order to specify the SQL analyser to use (issue #208)
- Make `project_root_path` and `verbosity` configurable from other setting source (issue #203)

## 4.0.0

- Drop support for Python 2.7, 3.5 and 3.6
- Add support for Python 3.10
- Drop support for Django 1.11, 2.0, 2.1, 3.0 and 3.1
- Add support for Django 4.0
- Fix index creation detection when table is being created in the transaction (issue #178)
- Handle unique index creation as adding a unique constraint (issue #183)
- Allow any option to be set/unset in config file (issue #167)
- Allow using Django settings for any option to be set/unset (issue #198)
- Raise when unsupported database vendor, allow passing an option to select SQL analyser (issue #138 and #169)

## 3.0.1

Fixed bug:
- Setting a field as NOT NULL without default passed the linter.

## 3.0.0

**Breaking API change on `lintmigrations` command**:
* the positional argument `GIT_COMMIT_ID` becomes an optional argument with the named parameter ` --git-commit-id [GIT_COMMIT_ID]`
* the `lintmigrations` command takes now two positional arguments: `lintmigrations [app_label] [migration_name]`

New features:
* raise warning when create or dropping an index in a non-concurrent manner using postgresql

Miscellaneous:
* Add complete and working support for `toml` configuration files
* Handle `--verbosity 0` or `-v 0` correctly to not print anything from the linter
* Add code coverage to the linter
* Renamed `master` branch to `main`

## 2.5.3

* Stop packaging the 'tests' module into the release wheel file
* Add Django 3.2 support

## 2.5.2

* Remove `toml` support for config files

## 2.5.1

* Remove `.editorconfig` from default config file
* Allow utf-8 encoding in config files

## 2.5.0

**Renamed lint checks**:
* `REVERSIBLE_DATA_MIGRATION` -> `RUNPYTHON_REVERSIBLE`
* `NAMING_CONVENTION_RUNPYTHON_ARGS` -> `RUNPYTHON_ARGS_NAMING_CONVENTION`
* `DATA_MIGRATION_MODEL_IMPORT` -> `RUNPYTHON_MODEL_IMPORT`
* `DATA_MIGRATION_MODEL_VARIABLE_NAME` -> `RUNPYTHON_MODEL_VARIABLE_NAME`
* `REVERSIBLE_RUNSQL_DATA_MIGRATION` -> `RUNSQL_REVERSIBLE`

Features/fixes:
* Add Python 3.9 support
* Make data migration model import error less strict (issue #121)
* Add warning detection on RunPython call when model variable name is not the same as model class name
* Run checks on RunSQL migration operations
* Rename the RunPython data migration lint checks for improved consistency
* Refactor and commonise the loggers, so that all modules the `django_migration_linter` logger
* Allow using configuration file for linter options
* Add option `--include-name` and `--include-name-contains` to only include migration with a certain name

Others:
* Switched from Travis CI to GitHub Actions for tests
* Apply `isort` to the project

## 2.4.1

* Add Django 3.1 support
* Upgrade linter versions of `flake8` and `black` to the latest

## 2.4.0

* Add possibility to lint newly generated migrations through the `makemigrations` command.
You can activate it through the `--lint` command option, or by default with the `MIGRATION_LINTER_OVERRIDE_MAKEMIGRATIONS = True`  Django settings.

## 2.3.0

* Handle making a column NOT NULL with a  Django default value as backward incompatible.
This should have been the case from the beginning, but it was not.
When one uses the `django-add-default-value` lib, the operation should still be compatible.
* Improve behaviour of the `--include-migrations-from` option.
When the given filename is missplelled, fail instead of linting all migrations.
When the file is empty, lint no migrations instead of linting all migrations.
* Update libraries used for testing (`tox`, `flake8`, etc.)

## 2.2.2

* Don't pin dependencies but only define minimal versions

## 2.2.1

Fixed bug:
* Correctly detect `apps.get_models` when on multiple lines in data migration

## 2.2.0

* Make summary output more friendly (Thanks to @vald-phoenix)
* Add forgotten documentation about `IgnoreMigration`
* Don't pin the `appdirs` strictly to 1.4.3, but allow higher versions

## 2.1.0

* Detect in data migrations if models are imported the correct way or not

## 2.0.0

* Add linting on data migrations that will raise warnings
* Add `--quiet warning` option to suppress warning messages
* Add `--warnings-as-errors` option to handle warnings as errors

## 1.5.0

* Rework documentation.
Make README smaller and clearer.
Move detailed documentation to docs/ folder.
* Add Django 3.0 to test matrix.

## 1.4.1

* Update testing dependencies (``mysqlclient``, Django 2.2 and ``black``)

## 1.4.0

* Add `--include-migrations-from` option to only consider migrations specified in a given file.
No matter if the migrations are selected from a git commit or not, we only select the ones in the given file.
* Add ``--quiet {ok,ignore,error}`` option to remove different or multiple types of messages from being printed to stdout.
* Make detection of problematic table and column a bit more resilient for UX.
* Add Python 3.8 to test matrix and update test dependencies.

Fixed bugs:

* Handle adding many to many fields correctly.

## 1.3.0

* Add `--exclude-migration-tests` option to ignore backward incompatible migration tests.
* Do not falsely detect dropping `NOT NULL`  constraint.
* Detect `DROP TABLE` backward incompatible migration.
* Detect adding a `UNIQUE` constraint as backward incompatible migration.
* Handle when Django tries to generate the SQL of a migration but raises an exception because
it cannot find the name of a certain constraint (because it has been dropped already).

Internal change:

* Differentiate different database vendors during SQL analysis.

## 1.2.0

* Add `--unapplied-migrations` and `--applied-migrations` mutually exclusive options
in order to respectively lint only unapplied or applied migrations.
* When loading migration starting from a git ref, cross the found migrations
with the currently loaded migrations in the project.
* Add `--project-root-path` which allows to specify the root path
which is used for finding the `.git` folder. (thanks to @linuxmaniac)

## 1.1.0

* Improve speed of ignored migration through the `IgnoreMigration`  operation.
Instead of generating their SQL, we verify if the Operation class is present.
Thus we don't have to wait for the SQL generation. Also improves the caching strategy.

Breaks some internal APIs, so minor update to 1.1.0.

## 1.0.0

**Breaking changes** of the linter usage. The linter now is a Django management command.
It should be used with a `python manage.py lintmigrations` followed by the usual options.
Additionally, the linter now needs to be added the to the `INSTALLED_APPS` in your Django settings.

* The linter is now much faster: we don't setup django once for each migration
* The linter is now more robust: we rely on Django internals to discover migrations
* Clean up of the testing setup: it is cleaner, less brittle and we have more confidence that it works
* Added support for Django 2.2
* Dropped support for Python 3.4

Fixed bugs:

* Made the cache database-dependent. Between multiple databases, some migrations would be considered correct while they are not on the used DB.
* Adding an erroneous migration to the ignore list on the second run would get the migration content from the cache and not ignore it.

## 0.1.5

* Bug fix: handle migration files with spaces in their name by quoting the migration name

## 0.1.4

* Explicitly test for mysql, sqlite3 and postgresql
* Fix alter column type detection in postgresql
* Do not create cache folder when -no-cache option
* Fix tests on Windows
* Use the same Python interpreter as used for launching the linter, instead of manually trying to re-create the path

## 0.1.3

* Fixes migrations that are ignored to generate empty SQL, that PostgreSQL complains about. It throws an exception making migrations fail, that are in fact valid. (Thanks to @fevral13 and @mes3yd)
* Add options -V and --version to show the current linter version

## 0.1.2

* Bug fix: handle when the linter is called in the working dir django-migration-linter .
* Bug fix: don't assume that the git root is at the same path as the django project

## 0.1.1

* Fix caching when app is in a folder and not at the root of the project
* Do not check for .git folder at root, because it might be located somewhere else. Let git itself do the job
* Make some imports relative

## 0.1.0

The django migration linter is now set to a Beta version, instead of Alpha since it has been used for quite some time.

Version 0.1.0 comes with:

* Possibility to ignore migration files entirely (without specifying them on the command line)
* This can be used by adding the migration operation IgnoreMigration() in the migrations operations list. See readme
* Caching. We cache failed and succeeded migrations (w.r.t. migration linting) in a cache file.
* Each migration file is stored as the MD5 hash of its content with the result of the linting

Other things:

* Added support for Python 3.7
* Added support for Django 2.1
* The codebase is now in black format to improve Python-code readability
* 2019 ! Happy new year

## 0.0.7

* Minor bug fix when no errors detected

## 0.0.6

* Use logger correctly
* Drop django 1.10 support

## 0.0.5

* Only consider added migrations during linting
* Change readme to rst format (for PyPi)
* Added examples folder
* Add django (at least 1.10) as requirement for the linter

## 0.0.4

Assure compatibility for multiple python and django versions.

* Try to improve PyPi readme
* Better error messages

## 0.0.3

* Made a mistake and accidentally deleted 0.0.2 from PyPi

## 0.0.2

* Python 3 compatibility
* Faster migration gathering

* Code base refactoring
* Test base refactoring

## 0.0.1

First version of command line tool to lint django migrations.
