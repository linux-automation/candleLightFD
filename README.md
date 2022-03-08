# PTX KiCad Template

## Install into KiCad

For a flatpak install of KiCad:

```
$ cd .var/app/org.kicad.KiCad/data/kicad/6.0/template

$ git clone {This}

```

## Use as template for a new project

Simply clone this git and:

```
$ git submoudle update --init
$ cd kicad-ptx-lib
$ git fetch
$ git checkout master
```
