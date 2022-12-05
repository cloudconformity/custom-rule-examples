# custom-rule-examples
An opensource collection of custom rule templates for Cloud One Conformity. Conformity Custom rules is an API-only feature for building Conformity rules that uses JSON rules engine and JSON path to run logic against cloud resource data ingested by Conformity.

The Conformity custom rules feature is currently [in preview](https://cloudone.trendmicro.com/docs/preview/).

For more information, please refer to:

- [Custom Rules API Reference](https://cloudone.trendmicro.com/docs/conformity/api-reference/tag/Custom-Rules#paths/~1custom-rules/get)
- [Conformity Custom Rules Overview](https://cloudone.trendmicro.com/docs/conformity/in-preview-custom-rules-overview/)
- [Getting Started with Conformity Custom Rules](https://cloudone.trendmicro.com/docs/conformity/in-preview-getting-started-with-custom-rules/)

## We :heart: Contributions
If you have ideas for custom rules, please feel free to submit a pull request and we can add it to our collection!

### Template formatting
When contributing, consider:
- adding some useful explanatory notes in your pull request
- a usable folder structure for easy navigation such as /Provider/Service/Rule.json. For example: /AWS/VPC/EnableVPCFlowLogs.json
- that your rules have well defined descriptions and clear structure, and adhere to the documentation provided above
- optional: save as templates for the /run endpoint and include the 'configuration' value and dummy resource data, e.g.

```bash
  {
    "configuration": {
        "name": "Unrestricted Inbound Firewall Rule on port XXXX",
        "description": "Firewall rules has ingress of 0.0.0.0/0 on port XXXX",
        "service": "CloudVPC",
        "resourceType": "cloudvpc-firewallrules",
        "severity": "HIGH",
        "enabled": true,
        "provider": "gcp",
        "categories": [
            "security"
        ],
        "remediationNotes": "Example rule template",
        "attributes": [
            {
                "name": "sourceRanges",
                "path": "data.sourceRanges[*]",
                "required": false
            }
        ],
        "rules": [
            {
                "conditions": {
                    "any": [
                        {
                            "fact": "sourceRanges",
                            "operator": "doesNotContain",
                            "value": "0.0.0.0/0"
                        }                       
                    ]
                },
                "event": {
                    "type": "Example rule template"
                }
            }
        ]
    },
    "resource": {
        "accountId": "933d1234-1234-1234-1234-1234ce55ce62",
        "organisationId": "12345234-6789-6789-6789-2345ce557890",
        "resourceId": "1234123412349999234",
        "service": "CloudVPC",
        "ccrn": "ccrn:gcp:933d1234-1234-1234-1234-1234ce55ce62:CloudVPC:global:1234123412349999234",
        "region": "global",
        "descriptorType": "cloudvpc-firewallrules",
        "data": {
            "id": "1234123412349999234",
            "name": "test-firewall-rule",
            "network": "XYZ"
            "sourceRanges": [
                "0.0.0.0/0",
                "10.0.0.0/0"
            ],
            "direction": "INGRESS"
        },
        "lastModifiedDate": 1650951283031,
        "lastModifiedBy": "SYSTEM"
    }
  }
```
- optional: save as clean templates for saving directly via the [Create Custom Rule](https://cloudone.trendmicro.com/docs/conformity/api-reference/tag/Custom-Rules#paths/~1custom-rules/post) POST request, e.g.

```bash
  {
        "name": "Unrestricted Inbound Firewall Rule on port XXXX",
        "description": "Firewall rules has ingress of 0.0.0.0/0 on port XXXX",
        "service": "CloudVPC",
        "resourceType": "cloudvpc-firewallrules",
        "severity": "HIGH",
        "enabled": true,
        "provider": "gcp",
        "categories": [
            "security"
        ],
        "remediationNotes": "Example rule template",
        "attributes": [
            {
                "name": "sourceRanges",
                "path": "data.sourceRanges[*]",
                "required": false
            }
        ],
        "rules": [
            {
                "conditions": {
                    "any": [
                        {
                            "fact": "sourceRanges",
                            "operator": "doesNotContain",
                            "value": "0.0.0.0/0"
                        }                       
                    ]
                },
                "event": {
                    "type": "Example rule template"
                }
            }
        ]
    }
```

### How to query resource data and start building
Before building a custom rule, first retrive the resource data structure.
The current version of custom rules allows you to query your cloud resource data using [the run endpoint](https://cloudone.trendmicro.com/docs/conformity/api-reference/tag/Custom-Rules#paths/~1custom-rules~1run/post).

Recommended: set up an API tool like [Postman](https://www.postman.com/)

1. Get check data for your chosen resource (see [List Account Checks](https://cloudone.trendmicro.com/docs/conformity/api-reference/tag/Checks#paths/~1checks/get))
2. Note the `resource` and `descriptorType` values from the check data
3. Use the custom rules `/run` command with `resourceDate=true` using the below request body ([Run Custom Rule](https://cloudone.trendmicro.com/docs/conformity/api-reference/tag/Custom-Rules#paths/~1custom-rules~1run/post)]. 

Ensure you update these values accordingly: 
`accountId`, `ApiKey`,
`provider`,`service`,`resourceType` (i.e. `descriptorType` from check data), `resourceId` (i.e. `resource` from check data)

```bash
curl -X POST https://conformity.{region}.cloudone.trendmicro.com/api/custom-rules/run?resourceData=true&accountId=<accountId> \
  -H "Content-Type: application/vnd.api+json" \
  -H "Authorization: ApiKey <ApiKey>" \
  -d 
{
    "configuration": {
        "provider": "aws",
        "service": "S3",
        "resourceType": "s3-bucket",
        "resourceId": "resource-id-xyz",
        "name": "Get Resource Data",
        "description": "Query resource data for a specific resource using the custom-rules run endpoint with resourceData=true",
        "remediationNotes": "Pass this template as a POST request body to /custom-rules/run?resourceData=true&accountId=aaa-bbb-ccc",
        "attributes": [
            {
                "name": "resourceId",
                "path": "resourceId",
                "required": true
            }
        ],
        "rules": [
            {
                "conditions": {
                    "all": [
                        {
                            "fact": "resourceId",
                            "operator": "notEqual",
                            "value": null
                        }
                    ]
                },
                "event": {
                    "type": "Resource ID found. Return resource data"
                }
            }
        ],
        "severity": "LOW",
        "enabled": true,
        "categories": [
            "operational-excellence"
        ]
    }
}
```
