---
title: Arbitrarily shaped wiboxes
permalink: /Arbitrarily_shaped_wiboxes/
---

This page documents the XShape extension. With this extension, one can manipulate two different shapes for a window. The *clipping shape* and the *bounding shape*.

Definition
----------

In the following examples, the left-most image shows the bounding shape used. In the middle is the clipping shape used and to the right there is the resulting wibox. The dark blue area is the wibox' border while the light blue area is the wibox' content where you can display anything (the border is always filled with the border color).

Let's start with the default values for both shapes:

[200px](/File:Shape-default.png "wikilink")

If we now modify the *bounding shape*, we get a round wibox:

[200px](/File:Shape-bound-circle.png "wikilink")

If we modify the *clipping shape* instead, the outline of the wibox is the same, but the border gets thicker at the edges:

[200px](/File:Shape-clip-circle.png "wikilink")

If we now modify both shapes, we can get a round wibox with a border:

[200px](/File:Shape-both-circle.png "wikilink")

As you can easily see, the *border_width* property of a wibox doesn't have any effect as soon as you start to use the shape extension.

Code
----

To modify some of a wibox' shape, you set the **shape_bounding** or **shape_clip** properties of a wibox to an image.

`-- Create a 30x30px image`
`local img = image.argb32(30, 30, nil)`
`-- Fill it with white (= not part of the shape)`
`img:draw_rectangle(0, 0, 30, 30, true, "#ffffff")`
`-- Draw a black circle to the center of the image`
`img:draw_circle(15, 15, 14, 14, true, "#000000")`
`-- Set the bounding shape of wibox "w" to our image`
`w.shape_bounding = img`

If you want to reset a shape to its default value, you can do this easily too:

`w.shape_bounding = nil`