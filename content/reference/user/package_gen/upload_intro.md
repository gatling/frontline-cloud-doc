To upload generated package, refer to the [packages section]({{< ref "../package_conf" >}}).

Alternatively, you can configure {param} to upload your package on Gatling Enterprise.

You'll need to [create a package]({{< ref "../package_conf" >}}), and [create an API token]({{< ref "../../admin/api_tokens/#managing-api-tokens" >}}) with at lease `Packages` permission on te package team.

API Token need to be configured as:
- `GATLING_ENTERPRISE_API_TOKEN` environment variable
- `gatling.enterprise.apiToken` java property