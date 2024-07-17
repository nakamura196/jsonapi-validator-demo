# jsonapi validator demo

## Usage

To try out the libraries mentioned, the following repository has been prepared.

https://github.com/nakamura196/jsonapi-validator-demo

### Installation

While using `nvm` is assumed, it is not mandatory.

```bash
git clone https://github.com/nakamura196/jsonapi-validator-demo
cd jsonapi-validator-demo
nvm i 22
nvm use 22
pnpm i
```

## Test

### Valid Example

```json
{
  "jsonapi": {
    "version": "1.0",
    "meta": {
      "links": {
        "self": {
          "href": "http://jsonapi.org/format/1.0/"
        }
      }
    }
  },
  "data": [
    {
      "type": "record",
      "id": "10_A0024853",
      "attributes": {
        "title": "Sample"
      }
    }
  ]
}
```

```bash
./node_modules/jsonapi-validator/bin/jsonapi-validator.js -f ./01_valid.json
01_valid.json is valid JSON API.
```

### Invalid Example: Unnecessary Properties

There is an unnecessary property named `aaa`.

```json
{
  "jsonapi": {
    "version": "1.0",
    "meta": {
      "links": {
        "self": {
          "href": "http://jsonapi.org/format/1.0/"
        }
      }
    }
  },
  "aaa": {
    "bbb": "ccc"
  },
  "data": [
    {
      "type": "record",
      "id": "10_A0024853",
      "attributes": {
        "title": "Sample"
      }
    }
  ]
}
```

```bash
./node_modules/jsonapi-validator/bin/jsonapi-validator.js -f ./02_invalid_additional_properties.json
Invalid JSON API.
  should NOT have additional properties.
  schemaPath: #/additionalProperties
  additionalProperty: aaa
  should NOT have additional properties.
  schemaPath: #/additionalProperties
  additionalProperty: aaa
  should NOT have additional properties.
  schemaPath: #/additionalProperties
  additionalProperty: data
  should have required property 'errors'.
  schemaPath: #/required
  missingProperty: errors
  should NOT have additional properties.
  schemaPath: #/additionalProperties
  additionalProperty: aaa
  should NOT have additional properties.
  schemaPath: #/additionalProperties
  additionalProperty: data
  should have required property 'meta'.
  schemaPath: #/required
  missingProperty: meta
  should match exactly one schema in oneOf.
  schemaPath: #/oneOf
```

### Invalid Example: Missing Required Property

Missing a required property `type`.

```json
{
  "jsonapi": {
    "version": "1.0",
    "meta": {
      "links": {
        "self": {
          "href": "http://jsonapi.org/format/1.0/"
        }
      }
    }
  },
  "data": [
    {
      "id": "10_A0024853",
      "attributes": {
        "title": "Sample"
      }
    }
  ]
}
```

```bash
./node_modules/jsonapi-validator/bin/jsonapi-validator.js -f ./03_invalid_missing_property.json
Invalid JSON API.
  should be object.
  schemaPath: #/type
  type: object
  should have required property 'type'.
  schemaPath: #/required
  missingProperty: type
  should be null.
  schemaPath: #/oneOf/2/type
  type: null
  should match exactly one schema in oneOf.
  schemaPath: #/oneOf
  should NOT have additional properties.
  schemaPath: #/additionalProperties
  additionalProperty: data
  should have required property 'errors'.
  schemaPath: #/required
  missingProperty: errors
  should NOT have additional properties.
  schemaPath: #/additionalProperties
  additionalProperty: data
  should have required property 'meta'.
  schemaPath: #/required
  missingProperty: meta
  should match exactly one schema in oneOf.
  schemaPath: #/oneOf
```
