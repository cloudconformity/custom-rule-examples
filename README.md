# custom-rule-examples
An opensource collection of custom rule templates for Cloud One Conformity. Conformity Custom rules is an API-only feature for building Confomrity rules that uses JSON rules engine and JSON path to run logic against cloud resource data ingested by Conformity.

For more information, please refer to:

- [Custom Rules API Reference](https://cloudone.trendmicro.com/docs/conformity/api-reference/tag/Custom-Rules#paths/~1custom-rules/get)
- [Conformity Custom Rules Overview](https://cloudone.trendmicro.com/docs/conformity/in-preview-custom-rules-overview/)
- [Getting Started with Conformity Custom Rules](https://cloudone.trendmicro.com/docs/conformity/in-preview-getting-started-with-custom-rules/)

## We :heart: contributions
If you have ideas for custom rules, please feel free to submit a pull request and we can add it to our collection!

### Template formatting
When contributing, consider:
- adding some useful explanatory notes in your pull request
- a usable folder structure for easy navigation such as /Provider/Service/Rule.json. For example: /AWS/VPC/EnableVPCFlowLogs.json
- that your rules have well defined descriptions and clear structure, and adhere to the documentation provided above
- optional: save as templates for the /run endpoint and include the 'configiuration' value and dummy resource data
- optional: save as clean templates for saving directly via the (Create Custom Rule)[https://cloudone.trendmicro.com/docs/conformity/api-reference/tag/Custom-Rules#paths/~1custom-rules/post] POST request
