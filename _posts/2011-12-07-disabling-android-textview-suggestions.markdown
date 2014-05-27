---
layout: post
published: true
title: Disabling Android TextView suggestions
excerpt: Android TextViews provide suggestions to the user as they type. Although
  the user can disable this feature in the system settings there are cases where the
  developer wants to always have suggestions disabled for certain TextViews.
category: programming
tags: android mobile
comments: []
---

Android TextViews provide suggestions to the user as they type. Although the user can disable this feature in the system settings there are cases where the developer wants to always have suggestions disabled for certain TextViews.

To achieve this, the documentation indicates that you should use:

XML:

{% highlight xml %}
<TextView android:inputType="textNoSuggestions" />
{% endhighlight %}

Java:

{% highlight java %}
TextView.setInputType(InputType.TYPE_TEXT_FLAG_NO_SUGGESTIONS)
{% endhighlight %}

Annoyingly some HTC devices ignore this option and so a common approach has been instead to use:

XML:

{% highlight xml %}
<TextView android:inputType="textVisiblePassword" />
{% endhighlight %}

Java:

{% highlight java %}
TextView.setInputType(InputType.TYPE_TEXT_VARIATION_VISIBLE_PASSWORD)
{% endhighlight %}

Although this does work, it will restrict the keyboard language on some HTC devices to English. A better approach, that appears to work across all devices, is:

XML:

{% highlight xml %}
<TextView android:inputType="textFilter" />
{% endhighlight %}

Java:

{% highlight java %}
TextView.setInputType(InputType.TYPE_TEXT_VARIATION_FILTER)
{% endhighlight %}
