---
title: "apostrophe-versions (module)"
layout: module
children:
  - browser-apostrophe-versions
  - browser-apostrophe-versions-editor
---
## Inherits from: [apostrophe-module](../apostrophe-module/index.html)

## Methods
### enableCollection(*callback*) *[api]*

### ensureIndexes(*callback*) *[api]*

### docAfterSave(*req*, *doc*, *callback*) *[api]*

### pruneOldVersions(*doc*, *callback*) *[api]*
Prune old versions so that the database is not choked
with them. If a version's time difference relative to
the previous version is less than 1/24th the time
difference from the newest version, that version can be
removed. Thus versions become more sparse as we move back
through time. However if two consecutive versions have
different authors we never discard them because
we don't want to create a false audit trail. -Tom
### revert(*req*, *version*, *callback*) *[api]*
Revert to the specified version. The doc need not be passed
because it is already in version._doc.
### find(*req*, *criteria*, *options*, *callback*) *[api]*
Searches for versions.

The callback is invoked like this:

callback(null, versions)

The most recent version is first in the array.

options.skip and options.limit may be used to paginate.
If options.compare is set, then for each version ._changes
will be set to an array of changes since the preceding version
in the result set.

Permissions for the document associated with the returned
versions are checked. Any attempt to retrieve multiple versions
from different documents will result in an error.

The ._doc property of each version is set to the document.
### compare(*req*, *doc*, *version1*, *version2*, *callback*) *[api]*
Compares two versions and returns a description of
the differences between them. The description is an
array of objects, which may contain nested `changes`
arrays when changed properties are areas or
schema arrays. version2 is assumed to be the
newer version. The doc is examined to determine what
schema to use.
## API Routes
### POST /modules/apostrophe-versions/versions-editor

### POST /modules/apostrophe-versions/compare

### POST /modules/apostrophe-versions/revert

