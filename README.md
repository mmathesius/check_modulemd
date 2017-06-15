# check_modulemd

This contains [avocado testing framework](http://avocado-framework.github.io/)
based tests to conduct various module checks.

* check_modulemd.py - check the validity of a modulemd file

## Setup

Install prerequisite RPMs if necessary:

* pdc-client
* python2-aexpect - dependency for python-avocado
* python2-avocado - avocado testing framework
* python2-modulemd - Module metadata manipulation library
* python2-requests - HTTP library
* python-enchant - spell checker library
* hunspell-en-US - English dictionary
* module-testing-framework - MTF support (read modulemd files from config)

## Running check_modulemd.py

Call avocado to run check_modulemd.py, providing the path to the modulemd file using
[avocado's parameter passing mechanism](http://avocado-framework.readthedocs.io/en/latest/WritingTests.html#accessing-test-parameters):

    avocado run ./check_modulemd.py --mux-inject 'run:modulemd:/path/to/modulemd.yaml'

In order to use a custom list of packaging jargon in the spell-checker, an optional additional parameter may be passed. This can be done by also providing the path to the jargon file on the command line:

    avocado run ./check_modulemd.py --mux-inject 'run:modulemd:/path/to/modulemd.yaml' 'run:jargonfile:/path/to/jargon.txt'

or by providing both the modulemd and jargonfile parameters using a multiplexer yaml file like the one used in this example:

    avocado run ./check_modulemd.py -m examples-testdata/params-checkmmd.yaml

For convenience during development of the test script, a wrapper script is
provided that simplifies passing the required parameter:

    ./run-checkmmd.sh [--debug] /path/to/modulemd.yaml

You can run project also as part of Modularity Testing Framework.
It will read modulemd files from config file and and can be scheduled as one of tests.
Jargon file can be passed via env variable JARGONFILE=/path/to/jargon.txt or
as multiplex value

     avocado run ./check_modulemd.py


You can also use provided Dockerfile:

    docker build --tag=$USER/check-modulemd .

And use it:

    docker run -ti -v $PWD/$MODULE.yaml:/$MODULE.yaml $USER/check-modulemd



### Example modulemd files

Some example modulemd files can be obtained from the following locations:

* http://pkgs.fedoraproject.org/cgit/modules/base-runtime.git/plain/base-runtime.yaml
* http://pkgs.fedoraproject.org/cgit/modules/testmodule.git/plain/testmodule.yaml
* https://pagure.io/modulemd/raw/master/f/spec.yaml

### Taskotron

This check is executed by [Taskotron](https://fedoraproject.org/wiki/Taskotron), see [results](https://taskotron.fedoraproject.org/resultsdb/results?&testcases=dist.modulemd). An example command to run this through Taskotron on your local machine is:

    runtask --item 'modules/dnf#70a99271d33db2bb35c79cf0bb9a2c62548646cc' --type dist_git_commit ./runtask.yml
