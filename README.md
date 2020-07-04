
# Dylan's archive of pictures and videos

Goal: use git annex and Amazon glacier for long-term cloud storage of my personal pictures.

## Initializing

Have git-annex

Have glacier cli 

```bash
pip install git+https://github.com/basak/glacier-cli.git
sudo ln -s $(which glacier-cli) /usr/local/bin/glacier 
```

```bash
export AWS_ACCESS_KEY_ID="08TJMT99S3511WOZEP91"
export AWS_SECRET_ACCESS_KEY="s3kr1t"
git annex initremote glacier type=glacier encryption=none
git annex add bigfile.tar
git annex copy bigfile.tar --to glacier
```

When pushing on github, alwasy push both branches!
```bash
git push origin master git-annex
```

On the new compter, also have annex, and glacier-cli 
```bash
pip install git+https://github.com/basak/glacier-cli.git
sudo ln -s $(which glacier-cli) /usr/local/bin/glacier 
```

Then clone the repository, and **reconstruct the inventory** 

```bash
git annex sync
glacier vault list
glacier vault sync <vaultname>
```
The name I am using is `FotoDylanAnn` , make sure it is there.

Then finally
```bash
git annex get thefilewithphotos.tar --from glacier
```
wait for half a day, then get it again. It should work... 

## GitHub and branches

This might help or not... but you may want to synch all the branches

Or you will get an error with `git push origin master git-annex`

```bash
git checkout git-annex
git branch --set-upstream-to=origin/git-annex
git pull
```

## Adding new files

First, makes sure the vault is there.

```bash
git annex sync
glacier vault list
glacier vault sync FotoDylanAnn
```

Then same as above
```bash
export AWS_ACCESS_KEY_ID="08TJMT99S3511WOZEP91"
export AWS_SECRET_ACCESS_KEY="s3kr1t"
git annex add bigfile.tar
git annex copy bigfile.tar --to glacier
```

Finally, you may also want to sync the external HD in a similar way (see local backup section)


## Local backup

For this, I just followed verbatim the instruction on the website [link here](https://git-annex.branchable.com/walkthrough/)

The commands I used are:
```bash
git clone ~/Documents/Dylan/PhotoBackups
cd PhotoBackups/
git pull
git annex init "dylan's portable HD"
git remote add blackshard ~/Documents/Dylan/PhotoBackups
git annex synch blackshard
git annex get Fotografie/FotoPisa2009A.tar 
```

## Cryptography ! 

Simple version: no key files
```bash
gpg --batch --passprhase "blah blah" -c mysecretfile
```
And back
```bash
gpg --decrypt mysecretfile.gpg
```

## And compression

Compression
```bash
tar -Jcvf mycompressedfiles.tar.xz files... 
```

Extraction
```bash
tar -Jxvf mycompressedfiles.tar.xz -c NewFolder/ 
```
