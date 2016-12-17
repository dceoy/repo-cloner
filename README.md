repo-cloner
===========

Bare cloner for repositories on GitHub

Usage
-----

1.  Install the required packages.

    ```sh
    # Ubuntu
    $ sudo apt-get -y install git rake

    # CentOS
    $ sudo yum -y install git rake

    # Fedora
    $ sudo dnf -y install git rake

    # Homebrew on macOS
    $ brew install git
    ```

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
