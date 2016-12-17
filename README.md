repo-cloner
===========

Bare cloner for repositories on GitHub

Usage
-----

1.  Create and edit `config.yml`.

    ```sh
    $ rake template # generate a template for `config.yml`
    $ vi config.yml # => edit
    ```

    Write repository names at `repos` in `config.yml`.

2.  Clone or update git repositories.

    ```sh
    $ rake  # equal to `rake mirror`
    ```
