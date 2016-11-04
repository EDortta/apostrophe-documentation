---
title: "apostrophe-schemas (module)"
layout: reference
module: true
namespaces:
  browser: true
children:
  - browser-apostrophe-schemas
  - browser-apostrophe-array-editor-modal
---
## Inherits from: [apostrophe-module](../apostrophe-module/index.html)
### `apos.schemas`
This module provides schemas, a flexible and fast way to create new data types
by specifying the fields that should make them up. Schemas power
[apostrophe-pieces](../apostrophe-pieces/index.html),
[apostrophe-widgets](../apostrophe-widgets/index.html), custom field
types in page settings for [apostrophe-custom-pages](../apostrophe-custom-pages/index.html)
and more.

A schema is simply an array of "plain old objects." Each object describes one field in the schema
via `type`, `name`, `label` and other properties.

See the [schema guide](../../tutorials/getting-started/schema-guide.html) for a complete
overview and list of schema field types. The methods documented here on this page are most often
used when you choose to work independently with schemas, such as in a custom project
that requires forms.


## Methods
### createRoutes() *[routes]*

### pushAssets()

### pushCreateSingleton()

### compose(*options*)
Compose a schema based on addFields, removeFields, orderFields
and, occasionally, alterFields options. This method is great for
merging the schema requirements of subclasses with the schema
requirements of a superclass. See the apostrophe-schemas documentation
for a thorough explanation of the use of each option. The
alterFields option should be avoided if your needs can be met
via another option.
### refine(*schema*, *_options*)
refine is like compose, but it starts with an existing schema array
and amends it via the same options as compose.
### toGroups(*fields*)
Converts a flat schema (array of field objects) into a two
dimensional schema, broken up by groups
### subset(*schema*, *fields*)
Return a new schema containing only the fields named in the
`fields` array, while maintaining existing group relationships.
Any empty groups are dropped. Do NOT include group names
in `fields`.
### newInstance(*schema*)
Return a new object with all default settings
defined in the schema
### subsetInstance(*schema*, *instance*)

### empty(*schema*, *object*)
Determine whether an object is empty according to the schema.
Note this is not the same thing as matching the defaults. A
nonempty string or array is never considered empty. A numeric
value of 0 is considered empty
### indexFields(*schema*, *object*, *texts*)
Index the object's fields for participation in Apostrophe search unless
`searchable: false` is set for the field in question
### convert(*req*, *schema*, *from*, *data*, *object*, *callback*)
Convert submitted `data`, sanitizing it and populating `object` with it
### export(*req*, *schema*, *to*, *object*, *object*, *callback*)
Export sanitized 'object' into 'object'
### joinDriver(*req*, *method*, *reverse*, *items*, *idField*, *relationshipsField*, *objectField*, *options*, *callback*)
Driver invoked by the "join" methods of the standard
join field types.

All arguments must be present, however relationshipsField
may be undefined to indicate none is needed.
### join(*req*, *schema*, *objectOrArray*, *withJoins*, *callback*)
Carry out all the joins in the schema on the specified object or array
of objects. The withJoins option may be omitted.

If withJoins is omitted, null or undefined, all the joins in the schema
are performed, and also any joins specified by the 'withJoins' option of
each join field in the schema, if any. And that's where it stops. Infinite
recursion is not possible.

If withJoins is specified and set to "false", no joins at all are performed.

If withJoins is set to an array of join names found in the schema, then
only those joins are performed, ignoring any 'withJoins' options found in
the schema.

If a join name in the withJoins array uses dot notation, like this:

_events._locations

Then the objects are joined with events, and then the events are further
joined with locations, assuming that _events is defined as a join in the
schema and _locations is defined as a join in the schema for the events
module. Multiple "dot notation" joins may share a prefix.

Joins are also supported in the schemas of array fields.
### addFieldType(*type*)
Add a new field type. The `type` object may contain the following properties:

### `name`

Required. The name of the field type, such as `select`. Use a unique prefix to avoid
collisions with future official Apostrophe field types.

### `converters`

Required. An object with  `csv` and `form` sub-properties, functions which are invoked for
CSV import and form submissions respectively. These are functions which accept:

`req, data, name, object, field, callback`

Sanitize the contents of `data[name]` and copy values
known to be safe to `object[name]`. Then invoke the callback.

`field` contains the schema field definition, useful to access
`def`, `min`, `max`, etc.

If `form` is the same as `csv` you may write:

form: 'csv'

To reuse it.

### `empty`

Optional. A function which accepts `field, value` and returns
true only if the field should be considered empty, for purposes of
deciding if the entire object is empty or not.

### `bless`

Optional. A function which accepts `req, field` and calls `self.apos.utils.bless`
on any schemas nested within `field`, so that editors are allowed to edit content. See
the implementation of the `areas` field type for an example.

### `index`

Optional. A function which accepts `value, field, texts` and pushes
objects containing search engine-friendly text onto `texts`, if desired:

```javascript
index: function(value, field, texts) {
  var silent = (field.silent === undefined) ? true : field.silent;
  texts.push({ weight: field.weight || 15, text: (value || []).join(' '), silent: silent });
}
```

Note that areas are *always* indexed.
### getFieldType(*typeName*)

### pageServe(*req*)
When a page is served to a logged-in user, make sure the session contains a blessing
for every join configured in schemas for doc types
### bless(*req*, *schema*)

## Nunjucks template helpers
### toGroups(*fields*)

### field(*field*)

## API Routes
### POST /modules/apostrophe-schemas/arrayEditor

### POST /modules/apostrophe-schemas/arrayItems

### POST /modules/apostrophe-schemas/arrayItem

