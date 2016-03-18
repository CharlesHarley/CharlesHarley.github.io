---
layout: post
published: true
title: UIButton in UITableViewCell has no highlight state
category: programming
tags: mobile ios
comments: []
---

I recently created a custom UITableViewCell with a UIButton as a subview. Pretty standard except that the highlight state of the button was only activated if you held your finger on the button for more than some fraction of a second. The solution is quite straight forward but is subtle enough to justify a quick blog post.

Table view cells are displayed using a UITableView which is a subclass of UIScrollView and as such inherits the ability to scroll. A scroll view intercepts touch events and has a short delay to check if the touch event is part of a scroll gesture. If it isn't then the touch event is passed onto the corresponding view. That explains the delay in the button highlight state.

We can remove the delay by setting `UIScrollView.delaysContentTouches = NO`. A side effect of this change is that scroll gestures cannot start from the buttons. Depending on how big your buttons are this might not be ideal. The work around for this is to override the `(BOOL)touchesShouldCancelInContentView:(UIView *)view` method in your UITableView subclass like so:

```objective-c
- (BOOL)touchesShouldCancelInContentView:(UIView *)view {
    // Because we set delaysContentTouches = NO, we return YES for UIButtons
    // so that scrolling works correctly when the scroll gesture
    // starts in the UIButtons.
    if ([view isKindOfClass:[UIButton class]]) {
        return YES;
    }

    return [super touchesShouldCancelInContentView:view];
}
```
