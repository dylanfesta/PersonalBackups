
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




