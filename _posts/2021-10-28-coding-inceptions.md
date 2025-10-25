---
title: "Coding Inceptions"
date: 2021-10-28
categories: hopeless-programming
---

After seeing the [Inception](https://www.imdb.com/title/tt1375666/) movie,
nothing seems _too meta_ for this guy. Here comes another round mental pushups
forced on us _fellow coders_.

In this project, it is quite common to leave some some data markers to let other
services know there is new data available. So, at the end of an 80 lines of code
method called _save_all()_, our friend throws these bad boys:

> in _some_serializer.py :: save_all()_

```python
if updated_ids:
    markers.mark_boxes_from_dataset(account, updated_ids)
    markers.mark_labels_from_dataset(account, updated_ids)

if updated_template_ids:
    markers.mark_labels_from_template(account, updated_template_ids)
```


I am sure I'm going to regret this, but let's have a look at them



### Down the rabbit hole


> in _markers.py_ module

```python
def mark_boxes_from_dataset(account, dataset_ids):
    _mark_from_dataset(account, dataset_ids, mark_to_load)
```


Yeah, a redirected function call, not a terrible practice after all, when you
want to keep some interfaces stable...

> Wait, where is that **mark_to_load** symbol coming from ?
> Oh, no biggie, it's another function in the same module (phew), here it is:
>
> ```python
> def mark_to_load(account: models.Account, ids: List[int]):
>     _mark_to_load(models.ProductsToLoad, account, ids)
> ```
>
> This dude has created a new model to trace the database updates? But I
> digress, we'll take on that another time.

So, passing a hard-coded model class to another local function using the same
name. This sounds almost reasonable, until you think this through: passing a
local function to another local function in the same module, which only calls a
third local function.

![This is some serious gourmet shit](/res/serious-gourmet.gif){:width="250px" height="141px"}


Are you still with me? Good, let's go back tracing the first function, following
that private function call:

```python
def _mark_from_dataset(account, ds_ids, mark_to_load):
    product_ids = Dataset.objects.filter(pk__in=ds_ids).values_list("product_id", flat=True)
    mark_to_load(account, product_ids)
```

Oh yeah, naming a callable parameter same as the local function being passed can
only bring some nice variations in your borring office life. Let's move on...

Finally, some database juices are flowing in and immediately the _so long_
carried function kicks in and I can't remember what it does...

> _F\*ck!_ Now I can't use the _Jump to definition_ shortcut that my editor so
> kindly provides... so scrolling up and I read the passed function:

```python
def mark_to_load(account: models.Account, ids: List[int]):
    _mark_to_load(models.ProductsToLoad, account, ids)
```


Another indirection in the _same_ module... 🤨 Like we might be paid by the line
of code soon.


```python
def _mark_to_load(model, account, product_ids):
    if not trial_ids:
        return
    model.objects.bulk_create(
        [model(account=account, product_id=product_id) for product_id in set(trial_ids)]
    )
```


> Great, that _set()_ conversion is a going to help a lot with the performance.


Luckily we hit the bottom, and now we can see what was so precious that needed to
be protected from those volatile external calls:

* a query of a **hard-coded** model name, and
* a bulk create of another **hard-coded** model name

Quite dissapointing, I know...

Now, let's trace the second marker function call:

```python
def mark_labels_from_dataset(account, ds_ids):
    _mark_from_dataset(account, ds_ids, mark_labels_to_load)

```

Sh\*t, we're thrown straight into [The rabbit hole](#the-rabbit-hole) again, but
this time with another flavor of those precious local functions.


```python
def mark_labels_to_load(account, ds_ids):
    # filter out any product_id with no label defined
    ds_ids = list(
        Product.objects.annotate(n_labels=Count("labels"))
        .filter(pk__in=ds_ids, n_labels__gt=0)
        .values_list("pk", flat=True)
    )
    _mark_to_load(models.ProductsToLoad, account, ds_ids)
```

Yup, the same hard-coded model class, and, just to spice things up, this time he
overrides the passed ids so we cannot remember what they are; products or
datasets? :man_facepalming: At least, this time we are redirected to the bottom
of the last rabbit hole.


And now the last marker:

```python
def mark_labels_from_template(account, template_ids):
    product_ids = (
        LabelTemplate.objects.filter(pk__in=template_ids).values_list("product_id", flat=True)
    )
    mark_labels_to_load(tenant, product_ids)
```

> Why would you store any product ids in the template model? I cannot imagine.


And I think we have seen this function just before, so we are at the end of our
trip. What a terrible journey.

I'll never recover from this!
