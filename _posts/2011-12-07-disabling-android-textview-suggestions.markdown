---
layout: post
published: true
title: Disabling Android TextView suggestions
category: programming
tags: android mobile
comments: []
---

Android TextViews provide suggestions to the user as they type. Although the user can disable this feature in the system settings there are cases where the developer wants to always have suggestions disabled for certain TextViews.

To achieve this, the documentation indicates that you should use:

XML:

```xml
<TextView android:inputType="textNoSuggestions" />
```

Java:

```java
TextView.setInputType(InputType.TYPE_TEXT_FLAG_NO_SUGGESTIONS)
```

Annoyingly some HTC devices ignore this option and so a common approach has been instead to use:

XML:

```xml
<TextView android:inputType="textVisiblePassword" />
```

Java:

```java
TextView.setInputType(InputType.TYPE_TEXT_VARIATION_VISIBLE_PASSWORD)
```

Although this does work, it will restrict the keyboard language on some HTC devices to English. A better approach, that appears to work across all devices, is:

XML:

```xml
<TextView android:inputType="textFilter" />
```

Java:

```java
TextView.setInputType(InputType.TYPE_TEXT_VARIATION_FILTER)
```
