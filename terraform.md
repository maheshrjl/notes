# 🔬 Terraform

{% hint style="info" %}
Install terraform autocomplete - \`**`` terraform -install-autocomplete` ``**
{% endhint %}

### format, validate & Init

* `terraform fmt` - Format code as per HCL standard
* `terraform validate` - validate code syntax
* `terraform validate -backend=false` - Validate code, skip [backend](https://www.terraform.io/cli/commands/validate) validation

### State File

* `terraform state list` - List resource in the state file
* `terraform state show [resource]` - Show properties of a resource
* `terraform show` - Show everything in state file
* `terraform state list` - List resource
* `terraform refresh` - Reads the current settings from all managed remote objects and updates the Terraform state file

### Applying changes

* `terraform apply -replace [resource]` - replace a resource on next run.
* `terraform apply -refresh-only` - Presents interactive prompt for you to confirm the detected changes to the state file

{% hint style="info" %}
Useful when we need to execute provisioners because terraform doesn't detect changes in provisionders. `terraform taint` was the command in old version
{% endhint %}

### Terraform Providers

* `terraform providers` - Get a list of currently installed providers

{% hint style="info" %}
Use alternate provider by setting an alias & refer the provider with `provider = alias`
{% endhint %}

### Visualization & Graph

* `terraform graph | dot -Tsvg > graph.svg` - Output visual execution graph of terraform resources in svg format&#x20;
