# Pulumi repository for Cloudflare settings

Figuring out how to use Pulumi, and moving my personal zone to be managed by
it. Uses a file store backend saved to the repository for now as I don't want
to depend on Pulumi cloud.

## Local setup

```
# Create local backend
export PULUMI_CONFIG_PASSPHRASE=<>
export CLOUDFLARE_API_TOKEN=<>
export CLOUDFLARE_ZONE=<>
pulumi login file://
pulumi stack init production
# Import existing resources from cloudflare
curl -H "Content-Type: application/json" -H "Authorization: Bearer $CLOUDFLARE_API_TOKEN" https://api.cloudflare.com/client/v4/zones/${CLOUDFLARE_ZONE}/dns_records |  jq '{"resources": [(.result | .[] | {"name": .name, "type": "cloudflare:index/record:Record", "id": "\(.zone_id)/\(.id)"})]}' > import.json
pulumi import -f import.json -o imported.yaml
```
