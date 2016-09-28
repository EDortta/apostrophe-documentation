---
title: "apostrophe-pieces-widgets (module)"
layout: reference
module: true
namespaces:
  browser: true
children:
  - browser-apostrophe-pieces-widgets-editor
  - browser-apostrophe-pieces-widgets
---
## Inherits from: [apostrophe-widgets](../apostrophe-widgets/index.html)
`apostrophe-pieces-widgets` provides widgets that display display pieces of a
particular type around the site.

You will `extend` this module in new modules corresponding to your modules
that extend `apostrophe-pieces`.

To learn more and see complete examples, see:

[Reusable content with pieces](../../tutorials/getting-started/reusable-content-with-pieces.html)


## Methods
### load(*req*, *widgets*, *callback*)
Load the appropriate pieces for each widget in the array. Apostrophe will try to feed
us as many at once as it can to cut down on database queries. We'll take all the
widgets for which pieces were chosen "by id" and do a single query, via
self.loadManyById. For everything we'll call self.loadOne individually, via
self.loadOthersOneAtATime. But in ALL cases, we invoke self.afterLoadOne for
each widget, allowing an opportunity to do custom work without thinking
about all this.
### loadManyById(*req*, *widgets*, *callback*)
Load many widgets, all of which were set to choose pieces "by id." This allows
Apostrophe to work efficiently when a page contains many pieces widgets in an
array, etc. This method is called by self.load, you don't need to call it yourself.

This method still calls afterLoadOne for each widget, so there is still a simple
way to go beyond this if you need to do something fancy after a widget has been
through the normal loading process.
### loadOthersOneAtATime(*req*, *widgets*, *callback*)
Load widgets that were NOT set to choose pieces "by id." Feeds them all
through self.loadOne and self.afterLoadOne. You don't have to call this,
self.load calls it for you.
### loadOne(*req*, *widget*, *callback*)
Load related content for a single widget. This method is invoked only
if the `loadManyById` option has been set to `false` for your subclass.
Doing so has performance costs when widgets are numerous.
### attachPiecesToWidget(*widget*, *pieces*)
Given an array of pieces, this method attaches them to the widget
as the _pieces property correctly with pushPiecesToWidget, and
orders them correctly if the user chose them in a specific order
### orderPiecesForWidget(*widget*)
A utility method that puts the pieces loaded for the widget in the
order requested by the user. widget._pieces should already be loaded
at this point. Called for you by the widget loader methods; useful
if you are overriding loadOne and disabling loadManyById
### pushPieceForWidget(*widget*, *piece*)
A utility method to append a piece to the ._pieces array for the given widget correctly,
whether the join has relationship properties or not.
### afterLoadOne(*req*, *widget*, *callback*)
This ALWAYS gets called, so you can do special handling here no matter what.
Whether your widgets were loaded en masse "by id" or one at a time, this method
always gets called for each.
### widgetCursor(*req*, *criteria*)
Hook to modify cursor before the load method is invoked. Applies the filters
specified for the join.
