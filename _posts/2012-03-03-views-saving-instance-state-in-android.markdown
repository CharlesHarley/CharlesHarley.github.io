---
layout: post
published: true
title: Views saving instance state in Android
category: programming
tags: android mobile
comments: []
---

The activity [lifecycle](http://developer.android.com/reference/android/app/Activity.html#ActivityLifecycle) is central to Android development. Although it is quite well defined, it does take a bit of getting used to. One aspect new developers perhaps struggle with is that rotating the device will by default destroy and recreate the activity.

A well behaved activity will restore its state after a rotation so the user can continue using the application with no lose of state. e.g. [EditText](http://developer.android.com/reference/android/widget/EditText.html) widgets should restore the text they contained when the device was rotated and a long running operation should resume without restarting.

There are a few topics to understand to achieve this effectively but one that I'd like to talk about is views saving and restoring their instance state. In most cases saving state is actually very simple but when creating compound views a couple extra steps are required. Compound views combine multiple views into a single convenient reusable view, e.g. the [NumberPicker](http://developer.android.com/reference/android/widget/NumberPicker.html).

## Saving individual views

For individual views, saving instance state is as simple as defining a unique [ID](http://developer.android.com/reference/android/view/View.html#attr_android:id "Android documentation on View IDs") for each view that you want to save its state. The activity will take care of the rest!

IDs only need to be unique within the activity where they are used.

## How saving instance state works

The instance state of each view within an activity is saved in shared persisted data with the state for each view referenced using the view's ID. Hence why the IDs need to be unique.

To save state the activity traverses the layout hierarchy and for each view it encounters it calls [View.saveHierarchyState()](http://developer.android.com/reference/android/view/View.html#saveHierarchyState(android.util.SparseArray<android.os.Parcelable>)) which in turn calls [View.dispatchSaveInstanceState()](http://developer.android.com/reference/android/view/View.html#dispatchSaveInstanceState(android.util.SparseArray<android.os.Parcelable>)). If the view has an ID this method calls [View.onSaveInstanceState()](http://developer.android.com/reference/android/view/View.html#onSaveInstanceState()), which saves its state to a [Parcelable](http://developer.android.com/reference/android/os/Parcelable.html) object and returns it. View.dispatchSaveInstanceState() then takes the Parcelable and saves it to the shared persisted data using the ID of the view.

Restoring state is a very similar process with the activity traversing the layout hierarchy and for each view it calls [View.restoreHierarchyState()](http://developer.android.com/reference/android/view/View.html#restoreHierarchyState(android.util.SparseArray<android.os.Parcelable>)) which in turn calls [View.dispatchRestoreInstanceState()](http://developer.android.com/reference/android/view/View.html#dispatchRestoreInstanceState(android.util.SparseArray<android.os.Parcelable>)). If the view has an ID this method retrieves the corresponding Parcelable from the shared persisted data and passes it through to [View.onRestoreInstanceState()](http://developer.android.com/reference/android/view/View.html#onRestoreInstanceState(android.os.Parcelable)), which then restores the view's state.

[ViewGroup](http://developer.android.com/reference/android/view/ViewGroup.html), which extends View, modifies the process slightly to pass the call to View.dispatchSaveInstanceState() onto its child views.

Simple really! :)

## Saving compound views

Saving instance state for compound views works in the same way as individual views but does require a little extra work when creating them to ensure that if an activity contains more than one instance of the same compound view, its state is correctly saved.

Compound views contain one or more child views and if an activity contains multiple instances of the same compound view then the IDs of the child views are no longer unique. To resolve this issue, the compound view class needs to take on the responsibility of saving and restoring the state of its child views.

To make it easier to understand I've created an [example project](https://github.com/CharlesHarley/Example-Android-SavingInstanceState) that defines a compound view that correctly saves and restores the state of its child views. The project contains various files to make the application work but if you want to skip to the important bit take a look at the [LockCombinationPicker](https://github.com/CharlesHarley/Example-Android-SavingInstanceState/blob/master/src/com/example/android/savinginstancestate/views/LockCombinationPicker.java) class. It contains 4 overridden methods that ensure the state of the child views are correctly saved and restored.

{% highlight java %}
@Override
protected Parcelable onSaveInstanceState() {
    Parcelable superState = super.onSaveInstanceState();
    return new SavedState(superState, numberPicker1.getValue(), numberPicker2.getValue(), numberPicker3.getValue());
}

@Override
protected void onRestoreInstanceState(Parcelable state) {
    SavedState savedState = (SavedState) state;
    super.onRestoreInstanceState(savedState.getSuperState());

    numberPicker1.setValue(savedState.getNumber1());
    numberPicker2.setValue(savedState.getNumber2());
    numberPicker3.setValue(savedState.getNumber3());
}

@Override
protected void dispatchSaveInstanceState(SparseArray container) {
    super.dispatchFreezeSelfOnly(container);
}

@Override
protected void dispatchRestoreInstanceState(SparseArray container) {
    super.dispatchThawSelfOnly(container);
}
{% endhighlight %}

LockCombinationPicker saves and restores the state of its children by overriding View.onSaveInstanceState() and View.onRestoreInstanceState() much as you would expect. In addition to this we need to ensure the compound view's children aren't then called to save their state as well. We do this by overriding View.dispatchSaveInstanceState() and View.dispatchRestoreInstanceState() and calling [ViewGroup.dispatchFreezeSelfOnly()](http://developer.android.com/reference/android/view/ViewGroup.html#dispatchFreezeSelfOnly(android.util.SparseArray<android.os.Parcelable>)) and [ViewGroup.dispatchThawSelfOnly()](http://developer.android.com/reference/android/view/ViewGroup.html#dispatchThawSelfOnly(android.util.SparseArray<android.os.Parcelable>)) respectively. These methods ensure that View.onSaveInstanceState() and View.onRestoreInstanceState() are called on the compound view class and not on the child views.

And that is it! To then save and restore the state of the compound view(s) within an activity just give it a unique ID.

[View example application on Github](https://github.com/CharlesHarley/Example-Android-SavingInstanceState)
