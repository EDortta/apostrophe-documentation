---
title: "apostrophe-attachments (module)"
layout: reference
module: true
namespaces:
  browser: true
children:
  - browser-apostrophe-attachments
  - browser-apostrophe-attachments-crop-editor
  - browser-apostrophe-attachments-focal-point-editor
---
## Inherits from: [apostrophe-module](../apostrophe-module/index.html)
### `apos.attachments`
This module implements the
[attachment](../../tutorials/getting-started/schema-guide.html#attachment) schema field type,
which makes it straightforward to allow users to attach uploaded files to docs. See
the [schema guide](../../tutorials/getting-started/schema-guide.html#attachment) for
more information.

## Options

### `addImageSizes`

Add an array of image sizes, in addition to Apostrophe's standard sizes. For example:

```javascript
[
  {
     name: 'tiny',
     width: 100,
     height: 100
  }
]
```

The resulting image *will not exceeed* either dimension given, and will preserve its
aspect ratio.

These extra sizes are then available as the `size` option to `apostrophe-images` widgets
and when calling `apos.attachments.url`.

### `imageSizes`

Like `addImageSizes`, but Apostrophe's standard sizes are completely replaced. Bear in mind
that certain sizes are used by Apostrophe's editing interface unless overridden. We recommend
using `addImageSizes`.

### `fileGroups`

Apostrophe will reject files that do not have extensions configured via `fileGroups`.
the default setting is:

```
[
  {
    name: 'images',
    label: 'Images',
    extensions: [ 'gif', 'jpg', 'png' ],
    extensionMaps: {
      jpeg: 'jpg'
    },
    // uploadfs should treat this as an image and create scaled versions
    image: true
  },
  {
    name: 'office',
    label: 'Office',
    extensions: [ 'txt', 'rtf', 'pdf', 'xls', 'ppt', 'doc', 'pptx', 'sldx', 'ppsx', 'potx', 'xlsx', 'xltx', 'csv', 'docx', 'dotx' ],
    extensionMaps: {},
    // uploadfs should just accept this file as-is
    image: false
  }
]
```

NOTE: adding another extension for `images` will not make web browsers
magically know how to show it or teach uploadfs how to scale it. Don't do that.

You may add extensions to the `office` fileGroup.


## Methods
### addFieldType() *[schemaField]*

### fieldTypePartial(*data*) *[schemaField]*

### indexer(*value*, *field*, *texts*) *[schemaField]*

### accept(*req*, *file*, *callback*) *[api]*
For backwards compatibility. Equivalent to calling `insert` with
the same three arguments.
### insert(*req*, *file*, *options*, *callback*) *[api]*
Insert a file as an Apostrophe attachment. The `file` object
should be an object with `name` and `path` properties. 
`name` must be the name the user claims for the file, while `path`
must be the actual full path to the file on disk and need not have
any file extension necessarily.

Note that when using Express to handle file uploads,
req.files['yourfieldname'] will be such an object as long as you
configure jquery fileupload to submit one per request.

The `options` argument may be omitted completely.
If `options.permissions` is explicitly set to `false`,
permissions are not checked.
### getFileGroup(*extension*) *[api]*

### crop(*req*, *_id*, *crop*, *callback*) *[api]*

### sanitizeCrop(*crop*) *[api]*

### clone(*req*, *source*, *callback*) *[api]*
Clones a file
### url(*attachment*, *options*) *[api]*
This method is available as a template helper: apos.attachments.url

Given an attachment object,
return the URL. If options.size is set, return the URL for
that size (one-third, one-half, two-thirds, full). full is
"full width" (1140px), not the original. For the original, don't pass size.
If the "uploadfsPath" option is true, an
uploadfs path is returned instead of a URL.
### first(*within*, *options*) *[api]*
This method is available as a template helper: apos.attachments.first

Find the first attachment referenced within any object with
attachments as possible properties or sub-properties.

For best performance be reasonably specific; don't pass an entire page or piece
object if you can pass page.thumbnail to avoid an exhaustive search, especially
if the page has many joins.

Returns the first attachment matching the criteria.

For ease of use, a null or undefined `within` argument is accepted.

Examples:

1. In the body please

apos.attachments.first(page.body)

2. Must be a PDF

apos.attachments.first(page.body, { extension: 'pdf' })

3. May be any office-oriented file type

apos.attachments.first(page.body, { group: 'office' })

apos.images.first is a convenience wrapper for fetching only images.

OPTIONS:

You may specify `extension`, `extensions` (an array of extensions)
or `group` to filter the results.
### all(*within*, *options*) *[api]*
This method is available as a template helper: apos.attachments.all

Find all attachments referenced within an object, whether they are
properties or sub-properties (via joins, etc).

For best performance be reasonably specific; don't pass an entire page or piece
object if you can pass piece.thumbnail to avoid an exhaustive search, especially
if the piece has many joins.

Returns an array of attachments, or an empty array if none are found.

For ease of use, a null or undefined `within` argument is accepted.

Examples:

1. In the body please

apos.attachments.all(page.body)

2. Must be a PDF

apos.attachments.all(page.body, { extension: 'pdf' })

3. May be any office-oriented file type

apos.attachments.all(page.body, { group: 'office' })

apos.images.all is a convenience wrapper for fetching only images.

OPTIONS:

You may specify `extension`, `extensions` (an array of extensions)
or `group` to filter the results.
### each(*criteria*, *limit*, *each*, *callback*) *[api]*
Iterates over all of the attachments that exist, processing
up to `limit` attachments at any given time.

If only 3 arguments are given the limit defaults to 1.

For use only in command line tasks, migrations and other batch operations
in which permissions are a complete nonissue. NEVER use on the front end.
### hasFocalPoint(*attachment*) *[api]*
Returns true if, based on the provided attachment object,
a valid focal point has been specified. Useful to avoid
the default of `background-position: center center` if
not desired.
### focalPointToBackgroundPosition(*attachment*) *[api]*
If a focal point is present on the attachment, convert it to
CSS syntax for `background-position`. No trailing `;` is returned.
The coordinates are in percentage terms.
### getFocalPoint(*attachment*) *[api]*
Returns an object with `x` and `y` properties containing the
focal point chosen by the user, as percentages. If there is no
focal point, null is returned.
### addTypeMigration() *[api]*

### pushAssets() *[browser]*

### pushCreateSingleton() *[browser]*

### initUploadfs(*callback*)

### enableCollection(*callback*)

### enableHelpers()

### addPermissions()

## Nunjucks template helpers
### url(*attachment*, *options*)
This method is available as a template helper: apos.attachments.url

Given an attachment object,
return the URL. If options.size is set, return the URL for
that size (one-third, one-half, two-thirds, full). full is
"full width" (1140px), not the original. For the original, don't pass size.
If the "uploadfsPath" option is true, an
uploadfs path is returned instead of a URL.
### first(*within*, *options*)
This method is available as a template helper: apos.attachments.first

Find the first attachment referenced within any object with
attachments as possible properties or sub-properties.

For best performance be reasonably specific; don't pass an entire page or piece
object if you can pass page.thumbnail to avoid an exhaustive search, especially
if the page has many joins.

Returns the first attachment matching the criteria.

For ease of use, a null or undefined `within` argument is accepted.

Examples:

1. In the body please

apos.attachments.first(page.body)

2. Must be a PDF

apos.attachments.first(page.body, { extension: 'pdf' })

3. May be any office-oriented file type

apos.attachments.first(page.body, { group: 'office' })

apos.images.first is a convenience wrapper for fetching only images.

OPTIONS:

You may specify `extension`, `extensions` (an array of extensions)
or `group` to filter the results.
### all(*within*, *options*)
This method is available as a template helper: apos.attachments.all

Find all attachments referenced within an object, whether they are
properties or sub-properties (via joins, etc).

For best performance be reasonably specific; don't pass an entire page or piece
object if you can pass piece.thumbnail to avoid an exhaustive search, especially
if the piece has many joins.

Returns an array of attachments, or an empty array if none are found.

For ease of use, a null or undefined `within` argument is accepted.

Examples:

1. In the body please

apos.attachments.all(page.body)

2. Must be a PDF

apos.attachments.all(page.body, { extension: 'pdf' })

3. May be any office-oriented file type

apos.attachments.all(page.body, { group: 'office' })

apos.images.all is a convenience wrapper for fetching only images.

OPTIONS:

You may specify `extension`, `extensions` (an array of extensions)
or `group` to filter the results.
### hasFocalPoint(*attachment*)
Returns true if, based on the provided attachment object,
a valid focal point has been specified. Useful to avoid
the default of `background-position: center center` if
not desired.
### getFocalPoint(*attachment*)
Returns an object with `x` and `y` properties containing the
focal point chosen by the user, as percentages. If there is no
focal point, null is returned.
### focalPointToBackgroundPosition(*attachment*)
If a focal point is present on the attachment, convert it to
CSS syntax for `background-position`. No trailing `;` is returned.
The coordinates are in percentage terms.
## API Routes
### POST /modules/apostrophe-attachments/upload

### POST /modules/apostrophe-attachments/crop
Crop a previously uploaded image, based on the `id` POST parameter
and the `crop` POST parameter. `id` should refer to an existing
file in /attachments. `crop` should contain top, left, width and height
properties.

This route uploads a new, cropped version of
the existing image to uploadfs, named:

/attachments/ID-NAME.top.left.width.height.extension

The `crop` object is appended to the `crops` array property
of the file object.
### POST /modules/apostrophe-attachments/crop-editor

### POST /modules/apostrophe-attachments/focal-point-editor

