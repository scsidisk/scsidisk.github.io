Using autoenv with python virtualenvs
======================================

22 Mar 2014

[Autoenv](https://github.com/kennethreitz/autoenv) will blow your mind,
or at least the github page indicates as such. It's a \*very\* useful
bit of software, but there are a few sharp edges to be aware of. The
first is that every .env in the tree below your directory are executed
on every cd. This means that a .env to deactivate your virtual env in a
root project path will deactivate when you cd around inside of your
project path, and then re-activate. This seems less then efficient to
me, so below is my solution. We only deactivate when you cd into the
workspaces environment and only activate when we cd into the project
env.

Install
---------

Install it easily:

#### Mac OS X Using Homebrew

    $ brew install autoenv
    $ echo "source $(brew --prefix autoenv)/activate.sh" >> ~/.bash_profile

#### Using pip

    $ pip install autoenv
    $ echo "source `which activate.sh`" >> ~/.bashrc

#### Using git

    $ git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
    $ echo 'source ~/.autoenv/activate.sh' >> ~/.bashrc

#### Using AUR

Arch Linux users can install [autoenv](https://aur.archlinux.org/packages/autoenv/) or [autoenv-git](https://aur.archlinux.org/packages/autoenv-git/) with their favorite AUR helper.

You need to source activate.sh in your bashrc afterwards:

    $ echo 'source /usr/share/autoenv/activate.sh' >> ~/.bashrc

Configuration
----------------

Before sourcing activate.sh, you can set the following variables:

- AUTOENV_AUTH_FILE: Authorized env files, defaults to ~/.autoenv_authorized
- AUTOENV_ENV_FILENAME: Name of the .env file, defaults to .env
- AUTOENV_LOWER_FIRST: Set this variable to flip the order of .env files executed

workspaces/.env
---------------


    BASE_PATH=`dirname "${BASH_SOURCE}"`
    PWD=`pwd`

    if [[ "${BASE_PATH}" == "${PWD}" ]]
    then
        declare -f -F deactivate &>/dev/null

        if [[ "${?}" == "0" ]]
        then
            deactivate
        fi
    fi

workspaces/project/.env
-----------------------


    BASE_PATH=`dirname "${BASH_SOURCE}"`
    PWD=`pwd`

    if [[ "${BASE_PATH}" == "${PWD}" ]]
    then
        if [[ -e venv/bin/activate ]]
        then
            source venv/bin/activate
        fi
    fi

