  # Chalo JSON Schema Pdhe

  This repository contains a **comprehensive JSON Schema** designed purely for **educational purposes**. The schema demonstrates nearly every major keyword from the **Draft-07** specification, providing a hands-on resource for understanding how to define, structure, and validate complex JSON data.

  ---

  ## JSON

  JSON Schema is a vocabulary that allows you to **annotate** and **validate** JSON documents. It acts as a blueprint, ensuring that a piece of JSON data adheres to a specified structure, data types, and set of constraints.

  ### 🧱 Building Blocks

  * A schema is itself a **JSON Object** defined by curly braces (`{}`).
  * It operates on a **key-value pair** basis.
  * **Keywords** (like `$schema`, `type`, `properties`) are used as **keys** to apply validation rules.

  ---

  ## Keywords

  These keywords define the context and identity of the schema:

  | Keyword | Type | Purpose | Example |
  | :--- | :--- | :--- | :--- |
  | **`$schema`** | `string` | **Defines the JSON Schema version** (or "dialect") being used. | `"http://json-schema.org/draft-07/schema#"` |
  | **`$id`** | `string` | A **unique identifier (URI)** for the schema. | `"https://example.com/full-manifest.schema.json"` |
  | **`title`** | `string` | A **short, descriptive title** for the schema. | `"This is my first schema"` |
  | **`description`** | `string` | A **detailed explanation** of the schema's purpose. | `"A complex schema..."` |
  | **`$comment`** | `string` | Notes for schema maintainers (**ignored** by validators). | `"This is my schema..."` |

  The most crucial validation keyword is **`type`**, which defines the expected data type of the JSON being validated.

  * **Syntax:** `"type": "object"` or `"type": ["string", "null"]`
  * **Note:** In our full example, the root instance is explicitly set to `"type": "object"`.

  ---

  ## Data types

  JSON Schema uses validation keywords specific to the type being defined.

  ### 1. Object Keywords (`type: "object"`)

  | Keyword | Purpose | Example |
  | :--- | :--- | :--- |
  | **`properties`** | Defines the expected fields (keys) in the object, where each value is its own schema. | See `system_id` in the full schema. |
  | **`required`** | An **array of strings** listing the properties that **must** be present. | `"required": ["system_id", "status_check"]` |
  | **`min/maxProperties`**| Restricts the minimum/maximum number of allowed properties. | `"minProperties": 1` |
  | **`additionalProperties`**| If `false`, prohibits any keys not listed in `properties` or `patternProperties`. | `"additionalProperties": false` |
  | **`patternProperties`** | Allows property names to be validated using **Regular Expressions**. | `"^flag_\\d+$"` (matches keys like `flag_1`, `flag_2`, etc.) |

  ### 2. Array Keywords (`type: "array"`)

  | Keyword | Purpose | Example |
  | :--- | :--- | :--- |
  | **`items`** | The schema that **all** elements in the array must conform to. | `"items": {"type": "string"}` |
  | **`min/maxItems`** | Restricts the size (length) of the array. | `"maxItems": 5` |
  | **`uniqueItems`** | If `true`, every item in the array must be unique. | `"uniqueItems": true` |
  | **`contains`** | Requires that **at least one** item in the array matches the sub-schema. | `{"const": "HIGH_PRIORITY"}` |

  ### 3. String Keywords (`type: "string"`)

  | Keyword | Purpose | Example |
  | :--- | :--- | :--- |
  | **`minLength/maxLength`**| Restricts the length of the string. | `"minLength": 8` |
  | **`pattern`** | A **regular expression** the string value must match. | `"pattern": "^[a-f0-9]{8,36}$"` |
  | **`format`** | Defines a **semantic constraint** (e.g., date, email, URI, UUID). | `"format": "uuid"` |

  ### 4. Numeric Keywords (`type: "number"` or `"integer"`)

  | Keyword | Purpose | Example |
  | :--- | :--- | :--- |
  | **`minimum/maximum`**| The inclusive lower/upper bound. | `"minimum": 1` |
  | **`exclusiveMinimum/Maximum`**| The exclusive lower/upper bound (value must be strictly greater/less than). | `"exclusiveMaximum": 11` |
  | **`multipleOf`** | The number must be evenly divisible by this value. | `"multipleOf": 1` |

  ---

  ## Conditional Structures Present in JSON

  These keywords allow for complex rules and schema reuse.

  ### Schema Composition

  These keywords combine multiple sub-schemas:

  * **`allOf`**: The data must satisfy **ALL** listed schemas.
  * **`anyOf`**: The data must satisfy **AT LEAST ONE** listed schema.
  * **`oneOf`**: The data must satisfy **EXACTLY ONE** listed schema.
  * **`not`**: The data must **NOT** satisfy the listed schema.

  ### Conditional Logic

  The **`if`** / **`then`** / **`else`** keywords apply validation based on a condition:

  * **`if`**: The condition (a sub-schema) to check.
  * **`then`**: The schema to apply if the `if` condition is **true**.
  * **`else`**: The schema to apply if the `if` condition is **false**.

  ### Schema Reuse

  * **`definitions`**: (or `$defs` in newer drafts) An area to define reusable sub-schemas.
  * **`$ref`**: Used to **reference** a schema defined elsewhere, typically within `definitions` (e.g., `#/definitions/Contact`).

  ---

  ## A detailed schema

  For your reference

  ```json
  {
    "$schema":"[http://json-schema.org/draft-07/schema#](http://json-schema.org/draft-07/schema#)",
    "$id":"[https://example.com/full-manifest.schema.json](https://example.com/full-manifest.schema.json)",
    "title": "This is my first schema",
    "description":"A complex schema which include complex object validation schema attributes as a key to properly validate things for prompt",
    "$comment": "This is my schema in which i am using all attributes present in json schema",
  
    "type":"object", // The root instance must be an object
    "default":{}, // default value if json is missing

    // --- Object-Specific Keywords ---
    "minProperties": 1,
    "maxProperties": 10,
    "required": ["system_id", "status_check"],
    "properties": {
      "system_id": {
        "type": "string",
        "description": "Unique identifier for the system.",
        "minLength": 8,
        "maxLength": 36,
        "pattern": "^[a-f0-9]{8,36}$",
        "format": "uuid" 
      },
      "version": {
        "type": ["number", "null"],
        "description": "The manifest version.",
        "const": 1.0
      },
      "status_check": {
        "type": "boolean",
        "readOnly": true,
        "examples": [true]
      },
      "retry_attempts": {
        "type": "integer",
        "minimum": 1,
        "exclusiveMaximum": 11,
        "multipleOf": 1
      },
      "parameters": {
        "type": "array",
        "description": "A list of configuration items.",
        "minItems": 1,
        "maxItems": 5,
        "uniqueItems": true,
        "items": { "type": "string" },
        "contains": { "const": "HIGH_PRIORITY" }
      },
      "contact_info": {
        "$ref": "#/definitions/Contact"
      }
    },
    
    "patternProperties": {
      "^flag_\\d+$": { "type": "boolean" }
    },
    
    "additionalProperties": false,
    
    // --- Composition Keywords ---
    "allOf": [ {"minProperties": 1} ],
    "anyOf": [ 
      {"properties": {"system_id": {"format": "uuid"}}},
      {"properties": {"system_id": {"format": "hostname"}}}
    ],
    "oneOf": [ 
      {"required": ["version"]},
      {"not": {"required": ["version"]}}
    ],
    "not": { "properties": {"retry_attempts": {"minimum": 100}} },
  
    // --- Conditional Keywords ---
    "if": { "properties": {"status_check": {"const": true}} },
    "then": {
      "properties": {"contact_info": {"required": ["email"]}},
      "required": ["contact_info"]
    },
    "else": { "properties": {"contact_info": {"required": []}} },
    
    // --- Structural/Reuse Keywords ---
    "definitions": {
      "Contact": {
        "type": "object",
        "description": "A reusable schema for contact details.",
        "properties": {
          "name": {"type": "string"},
          "email": {"type": "string", "format": "email", "writeOnly": true},
          "phone": {"type": ["string", "null"]}
        },
        "dependentRequired": { "name": ["email"] },
        "dependentSchemas": { 
          "email": {
            "properties": {"phone": {"minLength": 10}},
            "required": ["phone"]
          }
        }
      }
    }
  }
