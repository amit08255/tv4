## Basic

This example provides a typical minimum you are likely to see in JSON Schema. It contains:

* [`$id`](http://json-schema.org/draft/2020-12/json-schema-core.html#rfc.section.8.2.1) keyword
* [`$schema`](http://json-schema.org/draft/2020-12/json-schema-core.html#rfc.section.8.1.1) keyword
* [`title`](http://json-schema.org/draft/2020-12/json-schema-validation.html#rfc.section.9.1) annotation keyword
* [`type`](http://json-schema.org/draft/2020-12/json-schema-validation.html#rfc.section.6.1.1) instance data model
* [`properties`](http://json-schema.org/draft/2020-12/json-schema-core.html#rfc.section.10.3.2.1) validation keyword
* Three keys: `firstName`, `lastName` and `age` each with their own:
  * [`description`](http://json-schema.org/draft/2020-12/json-schema-validation.html#rfc.section.9.1) annotation keyword.
  * `type` instance data model (see above).
* [`minimum`](http://json-schema.org/draft/2020-12/json-schema-validation.html#rfc.section.6.2.4) validation keyword on the `age` key.

```json
{
  "$id": "https://example.com/person.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Person",
  "type": "object",
  "properties": {
    "firstName": {
      "type": "string",
      "description": "The person's first name."
    },
    "lastName": {
      "type": "string",
      "description": "The person's last name."
    },
    "age": {
      "description": "Age in years which must be equal to or greater than zero.",
      "type": "integer",
      "minimum": 0
    }
  }
}
```

**Data**

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 21
}
```

## Describing geographical coordinates.

This example introduces:

* [`required`](http://json-schema.org/draft/2020-12/json-schema-validation.html#rfc.section.6.5.3) validation keyword
* [`minimum`](http://json-schema.org/draft/2020-12/json-schema-validation.html#rfc.section.6.2.4) validation keyword
* [`maximum`](http://json-schema.org/draft/2020-12/json-schema-validation.html#rfc.section.6.2.2) validation keyword

```json
{
  "$id": "https://example.com/geographical-location.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Longitude and Latitude Values",
  "description": "A geographical coordinate.",
  "required": [ "latitude", "longitude" ],
  "type": "object",
  "properties": {
    "latitude": {
      "type": "number",
      "minimum": -90,
      "maximum": 90
    },
    "longitude": {
      "type": "number",
      "minimum": -180,
      "maximum": 180
    }
  }
}
```

**Data**

```json
{
  "latitude": 48.858093,
  "longitude": 2.294694
}
```

## Arrays of things

Arrays are fundamental structures in JSON -- here we demonstrate a couple of ways they can be described:

* An array of string values.
* An array of objects.

We also introduce the following with this example:

* [`$defs`](http://json-schema.org/draft/2020-12/json-schema-core.html#rfc.section.8.2.4) keyword
* [`$ref`](http://json-schema.org/draft/2020-12/json-schema-core.html#rfc.section.8.2.3.1) keyword

```json
{
  "$id": "https://example.com/arrays.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "description": "A representation of a person, company, organization, or place",
  "type": "object",
  "properties": {
    "fruits": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "vegetables": {
      "type": "array",
      "items": { "$ref": "#/$defs/veggie" }
    }
  },
  "$defs": {
    "veggie": {
      "type": "object",
      "required": [ "veggieName", "veggieLike" ],
      "properties": {
        "veggieName": {
          "type": "string",
          "description": "The name of the vegetable."
        },
        "veggieLike": {
          "type": "boolean",
          "description": "Do I like this vegetable?"
        }
      }
    }
  }
}
```

**Data**

```json
{
  "fruits": [ "apple", "orange", "pear" ],
  "vegetables": [
    {
      "veggieName": "potato",
      "veggieLike": true
    },
    {
      "veggieName": "broccoli",
      "veggieLike": false
    }
  ]
}
```

## NodeJS example

```js
const tv4 = require("./t4");

const schema = {
  "required": [ "latitude", "longitude" ],
  "type": "object",
  "properties": {
    "latitude": {
      "type": "number",
      "minimum": -90,
      "maximum": 90
    },
    "longitude": {
      "type": "number",
      "minimum": -180,
      "maximum": 180
    }
  }
};

const data = {
  "latitude": 48.858093,
  "longitude": 2.294694,
};

const data2 = {
  "latitude": 48.858093,
};

var valid = tv4.validate(data, schema);
var valid2 = tv4.validate(data2, schema);

console.log(valid);
console.log(valid2);
```
