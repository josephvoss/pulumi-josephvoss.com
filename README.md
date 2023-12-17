# Setup

pulumi login file://
pulumi stack init production
# Import existing resources from cloudflare
curl -H "Content-Type: application/json" -H "Authorization: Bearer $CLOUDFLARE_API_TOKEN" https://api.cloudflare.com/client/v4/zones/1dee01b2352a5b47fa244f4ac91beb3c/dns_records |  jq '{"resources": [(.result | .[] | {"name": .name, "type": "cloudflare:index/record:Record", "id": "\(.zone_id)/\(.id)"})]}' > import.json
pulumi import -f import.json -o imported.yaml
