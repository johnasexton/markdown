# terraform cloud
_(free edition)_

### Get terraform binary
* [doc for getting latest terraform binary](../desktop-tooling/terraform.md)

### Create sample terraform cloud
* clone repo
* run setup script
* provision demo fake resources

```bash
terraform login
cd ~/workspace/hashicorp
git clone https://github.com/hashicorp/tfc-getting-started.git
cd tfc-getting-started
./scripts/setup.sh
```
##### details:
* https://app.terraform.io/app/example-org-6d647a/workspaces/getting-started
* mock infra created: https://app.terraform.io/fake-web-services
