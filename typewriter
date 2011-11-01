#!/usr/bin/env bash

set -x

config() {
    TEMPLATE=
    while getopts "t:" opt $@; do
      case $opt in
        t)
            echo "activated t"
            TEMPLATE=$OPTARG
            ;;
        [?])
            echo "Invalid option: -$OPTARG" >&2
            usage
            ;;
      esac
    done

    if [ -z $TEMPLATE ]; then
        echo "$0: missing template!"
        usage
    fi
}

usage() {
    echo "TODO. Sorry!"
    exit 1
}

prepare() {
    git update-index --ignore-submodules --refresh &> /dev/null
    if [ $? -ne 0 ]; then
        echo "$0: git repository not clean!"
        exit 1
    fi

    git pull origin gh-pages &> /dev/null
    if [ $? -eq 0 ]; then
        existingBranch
    else
        newBranch
    fi

    echo "$0: gh-pages branch prepared"
}

existingBranch() {
    git checkout gh-pages &> /dev/null
    if [! -f .typewriter]; then
        echo "$0: gh-pages branch exists, but was not built by typewriter!"
        exit 1
    fi
    git ls-files -z | xargs -0 rm -f
}

newBranch() {
    git symbolic-ref HEAD refs/heads/gh-pages
    rm .git/index
    git clean -fdx
}

build() {
    BUILDDIR=$(mktemp -d -t typewriter)
    if [ $? -ne 0 ]; then
        echo "$0: can't create temporary build directory!"
        exit 1
    fi
    echo "$0: building in $BUILDDIR..."
    
    git clone $TEMPLATE $BUILDDIR

    $BUILDDIR/make clean
    cp README* $BUILDDIR/src
    $BUILDDIR/make build
    if [ $? -ne 0 ]; then
        echo "$0: build failed!"
        exit 1
    fi

    touch $BUILDDIR/build/.typewriter
    touch $BUILDDIR/build/.nojekyll

    echo "$0: build finished"
}

commit() {
    cp -R $BUILDDIR/build .

    git add -A
    git commit -m "typewriter build $(date -u +%Y-%m-%dT%H:%MZ)"
    #git push origin gh-pages
    #git checkout master
}

config $@
build
prepare
commit

trap 'rm -rf $BUILDDIR' EXIT
