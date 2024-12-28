repo-cloner
===========

Bare cloner for repositories on GitHub

Usage
-----

1.  Install Git and Rake.

2.  Check out `repo-cloner`.

    ```sh
    $ git clone https://github.com/dceoy/repo-cloner.git
    $ cd repo-cloner
    ```

3.  Create and edit `config.yml`.

    ```sh
    $ rake init     # generate a template for `config.yml`
    $ vi config.yml # => edit
    ```

    Write repository names at `repos` in `config.yml`.

4.  Clone or update git repositories.

    ```sh
    $ rake
    ```
