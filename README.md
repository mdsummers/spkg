# `spkg`

**Simple packaging for RPMs** <sub>...because not everybody has a lookaside cache.</sub>

## Usage

### Setup
Clone the repo and place `spkg` somewhere on your path.

Install some development tools that `spkg` makes use of
```
sudo yum install rpmdevtools yum-utils
```

### Got an srpm? `spkg init`
You can make a build environment from it.

```
$ spkg init somepackage-0.1-1.el7.src.rpm
...
New build directory under somepackage-0.1-1.el7.src
$ cd somepackage-0.1-1.el7.src
$ ls -p
checksums  somepackage.spec  sources/
```
This will import all locally referenced sources to the build environment. Remote sources (referenced by url) will be moved
to the rpm SOURCES dir, available for building. Typically these remote sources are binary files, so it makes sense not to keep them under version control. Instead, we create a `checksums` file with the sha256 sums of all remote sources. This is checked before build operations.

Do yourself a favour and put it the build environment under version control
```bash
git init
git add .
git commit -m "Initial commit for somepackage"
```

### No srpm? No problem - `spkg prep`
Grab (or write) a spec file and put it under a directory.

```
$ ls
otherpackage.spec
$ spkg prep
...
```
Any remote sources will be fetched, and the user will be prompted to confirm checksums. Local sources should be manually added to a directory `sources`.
```
$ tree .
.
├── checksums
├── otherpackage.spec
└── sources
    ├── localsource1
    ├── localsource2
    └── localsource3
```

### Enough already, let's build some rpms - `spkg build`
```spkg build```
That's it. In the background `build` will trigger any previous steps if it needs to. In an extreme example, it's possible to run `spkg build` from a file containing only a spec file, and end up with rpms being built.

## Compatibility
`spkg` has been tested on the following platforms:
* el7 (CentOS)

\*sadface\*

## What's missing?
* GPG signing hooks
* Building under `mock`
* Testing under other rhel variants
