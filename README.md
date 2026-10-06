# Dependabot composer PHP-floor reproduction

Minimal reproduction for a Dependabot regression in the `composer` ecosystem.

- `composer.json` (`"type": "project"`) requires `"php": "^7.0 || ^8.0"` and has one dev dependency, `phpstan/phpstan: ^2.1` (which requires PHP `^7.4|^8.0`).
- `composer.lock` pins `phpstan/phpstan` to 2.1.0; newer 2.x releases are available.
- There is no `config.platform.php`.

Expected: Dependabot resolves at PHP 7.0 (the lowest version allowed by `require.php`), hits `phpstan/phpstan … requires php ^7.4|^8.0`, raises its platform PHP and opens a PR updating `phpstan/phpstan`.

Actual: the error is not recognised as a missing platform requirement, so Dependabot logs "No update possible for phpstan/phpstan" and the job finishes successfully.
