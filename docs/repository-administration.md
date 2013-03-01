# Yum Repository Administration
Notes for repo administration used by Wendall Cada for release.

## Repository management scripts.
Two scripts exist for managing this repository, initrepo and updaterepo.
These are simple bash scripts that do some of the redundant work involved in
updating the repository.

### initrepo
Script for updating repo. Found in /scripts.

Example:
    initrepo -d fedora/18 -a "i686 i686/debuginfo x86_64 x86_64/debuginfo SRPMS" -s "Wendall Cada <wendallc@example.com>"

Creates the following structure and signs.

    |-- fedora
        |-- 18
            |-- i686
                |-- debuginfo
            |-- SRPMS
            |-- x86_64
                |-- debuginfo

## updaterepo
Script for updating repo after packages have been added. Rebuilds the
databases, etc. Found in /scripts

Example:
    updaterepo -d fedora/18 -a "i686 i686/debuginfo x86_64 x86_64/debuginfo SRPMS"

## Package Signing
All scripts are signed by Wendall Cada. Sig can be found in root of the
repository.

    $ vi ~/.rpmmacros
        %_signature gpg
        %_gpg_name Wendall Cada <wendallc@example.com>

    $ rpm --addsign foo.i386.rpm
        Enter pass phrase:
        Pass phrase is good.
        foo.i386.rpm:

    $ rpm --checksig foo.i386.rpm
        foo.i686.rpm: rsa sha1 (md5) pgp md5 OK

### Sign Package During Build
    $ rpmbuild -ba --sign foo.spec

### Exporting Key to File
    $ gpg --output repo.asc --export -a 42A5E447
