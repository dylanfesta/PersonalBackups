
# Dylan's archive of pictures and videos

Goal: use git annex and Amazon glacier for long-term cloud storage of my personal pictures.

## Instructions

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


