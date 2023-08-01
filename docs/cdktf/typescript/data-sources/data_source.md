---

<!-- Please do not edit this file, it is generated. -->
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "null_data_source Data Source - terraform-provider-null"
subcategory: ""
description: |-
  The null_data_source data source implements the standard data source lifecycle but does not
  interact with any external APIs.
  Historically, the null_data_source was typically used to construct intermediate values to re-use elsewhere in configuration. The
  same can now be achieved using locals https://developer.hashicorp.com/terraform/language/values/locals or the terraform_data resource type https://developer.hashicorp.com/terraform/language/resources/terraform-data in Terraform 1.4 and later.
---

# null_data_source

The `nullDataSource` data source implements the standard data source lifecycle but does not
interact with any external APIs.

Historically, the `nullDataSource` was typically used to construct intermediate values to re-use elsewhere in configuration. The
same can now be achieved using [locals](https://developer.hashicorp.com/terraform/language/values/locals) or the [terraform_data resource type](https://developer.hashicorp.com/terraform/language/resources/terraform-data) in Terraform 1.4 and later.

## Example Usage

```typescript
// Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
import { Construct } from "constructs";
import {
  Token,
  TerraformCount,
  propertyAccess,
  Fn,
  TerraformOutput,
  TerraformStack,
} from "cdktf";
/*
 * Provider bindings are generated by running `cdktf get`.
 * See https://cdk.tf/provider-generation for more details.
 */
import { Elb } from "./.gen/providers/aws/elb";
import { Instance } from "./.gen/providers/aws/instance";
import { DataNullDataSource } from "./.gen/providers/null/data-null-data-source";
class MyConvertedCode extends TerraformStack {
  constructor(scope: Construct, name: string) {
    super(scope, name);
    /*In most cases loops should be handled in the programming language context and 
    not inside of the Terraform context. If you are looping over something external, e.g. a variable or a file input
    you should consider using a for loop. If you are looping over something only known to Terraform, e.g. a result of a data source
    you need to keep this like it is.*/
    const blueCount = TerraformCount.of(Token.asNumber("3"));
    const blue = new Instance(this, "blue", {
      ami: "ami-0dcc1e21636832c5d",
      instanceType: "m5.large",
      count: blueCount,
    });
    /*In most cases loops should be handled in the programming language context and 
    not inside of the Terraform context. If you are looping over something external, e.g. a variable or a file input
    you should consider using a for loop. If you are looping over something only known to Terraform, e.g. a result of a data source
    you need to keep this like it is.*/
    const greenCount = TerraformCount.of(Token.asNumber("3"));
    const green = new Instance(this, "green", {
      ami: "ami-0dcc1e21636832c5d",
      instanceType: "m5.large",
      count: greenCount,
    });
    const values = new DataNullDataSource(this, "values", {
      inputs: {
        all_server_ids: Token.asString(
          Fn.concat([
            propertyAccess(green, ["*", "id"]),
            propertyAccess(blue, ["*", "id"]),
          ])
        ),
        all_server_ips: Token.asString(
          Fn.concat([
            propertyAccess(green, ["*", "private_ip"]),
            propertyAccess(blue, ["*", "private_ip"]),
          ])
        ),
      },
    });
    new TerraformOutput(this, "all_server_ids", {
      value: propertyAccess(values.outputs, ['"all_server_ids"']),
    });
    new TerraformOutput(this, "all_server_ips", {
      value: propertyAccess(values.outputs, ['"all_server_ips"']),
    });
    new Elb(this, "main", {
      instances: Token.asList(
        propertyAccess(values.outputs, ['"all_server_ids"'])
      ),
      listener: [
        {
          instancePort: 8000,
          instanceProtocol: "http",
          lbPort: 80,
          lbProtocol: "http",
        },
      ],
    });
  }
}

```

<!-- schema generated by tfplugindocs -->
## Schema

### Optional

- `hasComputedDefault` (String) If set, its literal value will be stored and returned. If not, its value defaults to `"default"`. This argument exists primarily for testing and has little practical use.
- `inputs` (Map of String) A map of arbitrary strings that is copied into the `outputs` attribute, and accessible directly for interpolation.

### Read-Only

- `id` (String, Deprecated) This attribute is only present for some legacy compatibility issues and should not be used. It will be removed in a future version.
- `outputs` (Map of String) After the data source is "read", a copy of the `inputs` map.
- `random` (String) A random value. This is primarily for testing and has little practical use; prefer the [hashicorp/random provider](https://registry.terraform.io/providers/hashicorp/random) for more practical random number use-cases.


<!-- cache-key: cdktf-0.17.1 input-c57aa183eb3faecd392a2666301466639e17a246180fc7127c0c9b366d16d65b -->