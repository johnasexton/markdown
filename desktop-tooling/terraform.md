# terraform binary on local

### Get terraform binary
* latest terraform binary: https://www.terraform.io/downloads.html
* unzip, add to $PATH & run version command

```bash
cd ~/Downloads/
mkdir tf
mv terraform_1.0.4_darwin_amd64.zip tf/
cd tf/
unzip terraform_1.0.4_darwin_amd64.zip
mkdir -p ~/scripts/backups
cp /usr/local/bin/terraform ~/scripts/backups
mv terraform /usr/local/bin/
which terraform
terraform -version
```
