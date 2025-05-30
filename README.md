[npm](https://www.npmjs.com/package/@TerraDharitri/sdk-nestjs-cli)

General-purpose CLI tool to assist in microservice development

# Installation

Ensure that Node.js is installed on your system and then run:

```bash
npm install @TerraDharitri/sdk-nestjs-cli -g
```

This will install the CLI globally so you can use it from anywhere in your terminal.

## Expand schema file

To expand a shorthand configuration schema into a JSON schema, use the expand command with the CLI. Specify the input file containing the shorthand schema and the desired output file for the JSON schema:

```bash
drtnest schema expand <inputfile> <outputfile>
```

Assuming you have a shorthand configuration schema in a file named config.yml:

```yaml
title: config
apps:
  api:
    urls:
      prefix: string
libs:
  common:
    urls:
      api: string
    redis:
      host: string
      port: integer
```

Run the command:

```bash
drtnest schema expand schema.yaml schema.json
```

This command will transform the shorthand schema in schema.yaml into a JSON schema and save it in schema.json. The resulting JSON schema will look like:

```json
{
  "title": "config",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "apps": {
      "type": "object",
      "properties": {
        "api": {
          "type": "object",
          "properties": {
            "urls": {
              "type": "object",
              "properties": {
                "prefix": {
                  "type": "string"
                }
              },
              "required": ["prefix"],
              "additionalProperties": false
            }
          },
          "required": ["urls"],
          "additionalProperties": false
        }
      },
      "required": ["api"],
      "additionalProperties": false
    },
    "libs": {
      "type": "object",
      "properties": {
        "common": {
          "type": "object",
          "properties": {
            "urls": {
              "type": "object",
              "properties": {
                "api": {
                  "type": "string"
                }
              },
              "required": ["api"],
              "additionalProperties": false
            },
            "redis": {
              "type": "object",
              "properties": {
                "host": {
                  "type": "string"
                },
                "port": {
                  "type": "integer"
                }
              },
              "required": ["host", "port"],
              "additionalProperties": false
            }
          },
          "required": ["urls", "redis"],
          "additionalProperties": false
        }
      },
      "required": ["common"],
      "additionalProperties": false
    }
  },
  "required": ["apps", "libs"],
  "additionalProperties": false
}
```

## Generate schema file from configuration YAML

Use the generate command to create a YAML schema from a YAML configuration file that contains concrete values. This command infers the types from the actual values in the YAML file:

```bash
drtnest schema generate <inputfile> <outputfile>
```

Assuming you have a YAML configuration file named config.yaml:

```yaml
apps:
  api:
    urls:
      prefix: 'drt-microservice'
libs:
  common:
    urls:
      api: ${API_URL}
    redis:
      host: '127.0.0.1'
      port: 6379
```

Run the command:

```bash
drtnest schema generate config.yaml schema.yaml
```

This command will infer the types from the values provided in config.yaml and generate a YAML schema like this:

```yaml
title: config
apps:
  api:
    urls:
      prefix: string
libs:
  common:
    urls:
      api: string
    redis:
      host: string
      port: integer
```

## Generate TypeScript Interfaces

Use the `schema types` command to generate TypeScript interfaces from a YAML configuration schema. This command converts the schema into TypeScript interfaces, facilitating type safety in development.

```bash
drtnest schema types <inputfile> <outputfile>
```

Assuming you have a YAML configuration schema named schema.yaml:

```yaml
title: config
apps:
  api:
    urls:
      prefix: string
libs:
  common:
    urls:
      api: string
    redis:
      host: string
      port: integer
```

Run the command:

drtnest schema types schema.yaml types.ts

This command will generate TypeScript interfaces in types.ts based on the schema provided in schema.yaml. The resulting TypeScript file will look like this:

```typescript
/* Autogenerated code */

export interface Config {
  apps: {
    api: {
      urls: {
        prefix: string;
      };
    };
  };
  libs: {
    common: {
      urls: {
        api: string;
      };
      redis: {
        host: string;
        port: number;
      };
    };
  };
}
```
