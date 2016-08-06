---
title: "apostrophe-pages (module)"
children:
  - server-apostrophe-pages-cursor
  - browser-apostrophe-pages
  - browser-apostrophe-pages-chooser
  - browser-apostrophe-pages-editor
  - browser-apostrophe-pages-editor-update
  - browser-apostrophe-pages-reorganize
---
## Inherits from: [apostrophe-module](../apostrophe-module/index.html)

## Methods
### afterInit(*callback*, *criteria*, *projection*)
Wait until the last possible moment to add
the wildcard route for serving pages, so that
other routes are not blocked
### insert(*req*, *parentOrId*, *page*, *callback*) *[api]*
Insert a page as a child of the specified page or page ID.
### newChild(*parentPage*) *[api]*
This method creates a new object suitable to be inserted
as a child of the specified parent via insert(). It DOES NOT
insert it at this time. If the parent page is locked down
such that no child page types are permitted, this method
returns null.
### allowedChildTypes(*page*) *[api]*

### move(*req*, *movedId*, *targetId*, *position*, *callback*) *[api]*
Move a page already in the page tree to another location.
position can be 'before', 'after' or 'inside' and
determines the moved page's new relationship to
the target page.

The callback receives an error and, if there is no
error, also an array of objects with _id and slug
properties, indicating the new slugs of all
modified pages.
### moveToTrash(*req*, *_id*, *callback*) *[api]*
Accepts `req`, `_id` and `callback`.

Delivers `err`, `parentSlug` (the slug of the page's
former parent), and `changed` (an array of objects with
_id and slug properties, including all subpages that
had to move too).
### update(*req*, *page*, *callback*) *[api]*
Update a page. Currently just a wrapper for docs.update.
### park(*pageOrPages*) *[api]*
Ensure the existence of a page or array of pages and
lock them in place in the page tree.

The `slug` property must be set. The `parent`
property may be set to the slug of the intended
parent page, which must also be parked. If you
do not set `parent`, the page is assumed to be a
child of the home page, which is always parked.
See also the `park` option; typically invoked via
that option when configuring the module.
### getManager(*type*) *[api]*
Wraps docs.getManager, but also ensures that the schema includes the
minimum properties for a page. Since Apostrophe is not strict about requiring
all page types currently valid on the site to be enumerated, this is
a critical requirement for methods that implement page settings UI.
### setManager(*type*, *manager*) *[api]*
Currently a wrapper for docs.setManager.
### serve(*req*, *res*) *[api]*
self.pageAfterMove = function(req, moved, info, callback) {
  // eventually invoke callback
};
Route that serves pages. See afterInit in
index.js for the wildcard argument and the app.get call
### serveGetPage(*req*, *callback*) *[api]*

### serveLoaders(*req*, *callback*) *[api]*

### serveNotFound(*req*, *callback*) *[api]*
self.serveSecondChanceLogin = function(req, callback) {
  if (options.secondChanceLogin === false) {
    return setImmediate(callback);
  }
  if (req.user) {
    return setImmediate(callback);
  }
  if (req.data.page) {
    return callback(null);
  }
  // Try again without permissions. If we get a better page,
  // note the URL in the session and redirect to login.
  var slug = req.params[0];
  var req = self.apos.
  var cursor = self.find(req, { slug: slug })
    .permission(false);
  self.matchPageAndPrefixes(cursor, slug);
  return cursor.toObject(function(err, page) {
    if (page || (bestPage && req.bestPage && req.bestPage.slug < bestPage.slug)) {
      res.cookie('aposAfterLogin', req.url);
      return res.redirect('/login');
    }
  })
    return callback(null);
  });
  }
};
### serveDeliver(*req*, *err*) *[api]*

### beforeSendPage(*req*, *callback*) *[api]*
This method sets `req.data.home`. It is called automatically every
time `self.sendPage` is called in any module, which includes normal CMS pages,
404 pages, the login page, etc.

This allows non-CMS pages like `/login` to "see" `data.home` and `data.home._children`
in their templates.

For performance, if req.data.page is already set and it contains a
`req.data._ancestors[0]._children` property, that information
is leveraged to avoid redundant queries. If not, a query is made.

For consistency, the home page is always retrieved using the same filters that
are configured for `ancestors`. Normally that includes children of each
ancestor. If that is explicitly reconfigured without the `children` option,
you will not get `data.home._children`.
### isFound(*req*) *[api]*
A request is "found" if it should not be
treated as a "404 not found" situation
### getServePageFilters() *[api]*

### matchPageAndPrefixes(*cursor*, *slug*) *[api]*

### evaluatePageMatch(*req*) *[api]*

### ensureIndexes(*callback*) *[api]*

### pruneCurrentPageForBrowser(*page*) *[api]*
A limited subset of page properties is pushed to
browser-side JavaScript. If you want more you
should make your own req.browserCalls
### docFixUniqueError(*req*, *doc*) *[api]*
Invoked via callForAll in the docs module
### updateDescendantsAfterMove(*req*, *page*, *originalPath*, *originalSlug*, *callback*) *[api]*

### implementParkAll(*callback*) *[api]*

### implementParkOne(*req*, *item*, *callback*) *[api]*

### unparkTask(*callback*) *[api]*

### mapMongoIdToJqtreeId(*changed*) *[api]*
Routes use this to convert _id to id for the
convenience of jqtree
### docUnversionedFields(*req*, *doc*, *fields*) *[api]*
Invoked by the apostrophe-versions module.
Identify fields that should never be rolled back
### isPage(*doc*) *[api]*
Returns true if the doc is a page in the tree
(it has a slug with a leading /).
### isAncestorOf(*possibleAncestorPage*, *ofPage*) *[api]*
Returns true if `possibleAncestorPage` is an ancestor of `ofPage`.
A page is not its own ancestor. If either object is missing or
has no path property, false is returned.
### setManagerForUnspecifiedPageType() *[api]*
Set the manager object for "apostrophe-page", the general case in which we're interested
in all "regular pages" in the tree. Useful when you want to build navigation using
schema joins
## Nunjucks template helpers
### menu(*options*)

### publishMenu(*options*)

### isAncestorOf(*possibleAncestorPage*, *ofPage*)

## API Routes
### POST /modules/apostrophe-pages/editor
Render the editor for page settings
### POST /modules/apostrophe-pages/fetch-to-insert
Fetch data needed to edit and ultimately insert a page
### POST /modules/apostrophe-pages/insert

### POST /modules/apostrophe-pages/fetch-to-update
Fetch data needed to edit and ultimately update a page
### POST /modules/apostrophe-pages/update

### POST /modules/apostrophe-pages/move

### POST /modules/apostrophe-pages/move-to-trash

### POST /modules/apostrophe-pages/get-jqtree

### POST /modules/apostrophe-pages/reorganize

### POST /modules/apostrophe-pages/info

