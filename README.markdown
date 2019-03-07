# BioD

BioD is an open source library for computational biology and bioinformatics.

# Running locally and Deploying

```sh
git clone https://github.com/biod/biod.github.io
cd biod.github.io/
git checkout source
rake preview
rake generate
rake deploy
```

Note: Don't push any changes to master unless using `rake deploy`.
