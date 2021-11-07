# hopeless programming


If you want to learn how to be a Rick Sanchez of software development, buoy
you're in the right place. It is not really hard, you just need to write your
code in a bit more complicated way than it needs to be.

Maybe you can find some generalisation that will allow you to reuse three lines
of code in two places by creating a method that accepts five parameters?!

Maybe you can reduce three lines of code into one by using a clever triple nested
ternary operator?!

Maybe you can use chained functions, nested conditional statements, over-bloated
design patterns and clever one-liners that use niche hipster features of the
language you write in.

The sky is the limit! Of course, it will make the application harder to read and
maintain but that will probably be your coworkers’ problem, right? Excellent!

So give it a go and write a code that proves you’re a real hacker...



---


## The _code_ must go on


While I was hunting a minor bug in a front-end component, that arranges some
objects in a simple grid following various patterns, I found this precious gem,
which appears to be _so carefully crafted_:

```typescript
let i = iStart;
let j = jStart;
while (i < nColumns && j < nRanges && i >= 0 && j >= 0) {
    let plot = this.plot(i, j);
    if (plot) {
        this.sorted.push(plot);
    }
    if (walkOrder === 'left_right') {
        i += iDir;
        if (i >= nColumns || i < 0) {
            i = iStart;
            j += jDir;
        }
    } else if (walkOrder === 'top_down') {
        j += jDir;
        if (j >= nRanges || j < 0) {
            j = jStart;
            i += iDir;
        }
    } else if (walkOrder === 'zig_zag_horizontal') {
        if (j % 2 === jStart % 2) {
            i += iDir;
            if (i >= nColumns || i < 0) {
                i = iEnd;
                j += jDir;
            }
        } else {
            i -= iDir;
            if (i < 0 || i >= nColumns) {
                i = iStart;
                j += jDir;
            }
        }
    } else if (walkOrder === 'zig_zag_vertical') {
        if (i % 2 === iStart % 2) {
            j += jDir;
            if (j >= nRanges || j < 0) {
                j = jEnd;
                i += iDir;
            }
        } else {
            j -= jDir;
            if (j < 0 || j >= nRanges) {
                j = jStart;
                i += iDir;
            }
        }
    } else if (walkOrder === '2rows_horizontal') {
        if (j % 4 === jStart % 4) {
            j += jDir;
            if (j >= nRanges || j < 0) {
                j = jEnd;
                i += iDir;
            }
        } else if (j % 4 === (jStart + jDir) % 4) {
            i += iDir;
            if (i >= nColumns || i < 0) {
                i = iEnd;
                j += jDir;
            } else {
                j -= jDir;
            }
        } else if (j % 4 === (jStart + 2 * jDir) % 4) {
            j += jDir;
            if (j >= nRanges || j < 0) {
                j = jEnd;
                i -= iDir;
            }
        } else {
            i -= iDir;
            if (i >= nColumns || i < 0) {
                i = iStart;
                j += jDir;
            } else {
                j -= jDir;
            }
        }
    }
}
```


If this is not a _brain fart_, then I don't know what is. Clearly he is aware of
the nightmare he just created, but compensates with some clean coding discipline,
trying to keep the things readable, but obviously he has no clue how to really
do it. Still, that's no reason to stop and think things through.

> -- Dude, have you ever heard of higher level concepts? You code like you're
> working out, just to burn calories!


Now, he's not working with us anymore (what a surprise) and I have to fix the
bug in that apparently clean mess. :poop:

> Yay, go me!



---


## True hipsters never let frameworks to handle timezones


The next gem (worthy to be printed on our daily toilet paper) was found in a
Django project, fully packed with REST framework and all third party libraries
known to man. You should really struggle to write any low level processing code
these days, (almost) everything was solved by people with much more experience
and they also had the decency to share it with everyone for _free_.

It is worth knowing that in this project, our users are spread on all continents
(except the poles), and they are sending data to our central server every day.
So dealing with time zones is not really a thing far into the future, we need
this solved _yesterday_!

So, how do we go about adding such a feature?
Maybe [ISO standards](https://en.wikipedia.org/wiki/ISO_8601), or maybe use
[strftime()](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior),
or even REST framework serializer's [DateTimeField](https://www.django-rest-framework.org/api-guide/fields/#datetimefield)? Naah that's too mainstream for my hipster dude, he
prefers to serialize the timestamps on his own terms:


```python
def serialize_datetime(value, bigquery_compatibility=False):
    if value is None:
        return value

    assert value.tzinfo is None

    if bigquery_compatibility:
        return value.strftime("%Y-%m-%d %H:%M:%S.%f")[:-3]
    else:
        return "{}Z".format(value.strftime("%Y-%m-%dT%H:%M:%S.%f")[:-3])
```


_Rule of thumb_: Never trust an **if**, it forces you to deal with one maybe two
situation(s)... I hear they are highly criticized lately, use **assert**s instead.

If you read that assert carefully, you will see that he is aware of his doings,
but simply did not land on the right [stackoverflow](https://stackoverflow.com/)
post.

> As Albert Einstein once said: "Two things are infinite: the universe and human
> stupidity; and I'm not sure about the universe."


---


## Data guys cannot write code

This dude had no friends in the coding business. I say "no friends", but *no
acqauintances* would be more precise; no one in his circle to talk about his
misery...

I share this sh\*tty piece with you untouched, because I want you to know this
is for real, also I am afraid his madness is contagious... So *Ctrl-C + Ctrl-V*:

```python
class DimensionCreatedOnDevice(BaseModel):
    """
    Note that there is no data migration:
    only records created after this model will be marked.

    Currently this is used only by products with a manual layout,
    and it was introduced together with that layout (hence why the data migration
    is missing).
    """

    product = models.ForeignKey("Product", on_delete=models.CASCADE)
    dimension = models.ForeignKey("Dimension", on_delete=models.CASCADE)

    class Meta(BaseModel.Meta):
        unique_together = ("product", "dimension")
```

The comment says it clearly: this is not an accident, nor an experiment. You may
notice the clean coding discipline: clear and simple names, using whitespace in
code and comments and finally some comments that mention **why** the code is
there (woohoo!). So this is not happening in his first year, but clearly he has
reached his limits in this domain...

Oh, I get it, he's an SQL guy who got lost among coders...

> -- I have just one meme for you mate:
>
> ![booleans](./res/booleans.jpeg){:width="300px" height="214px"}


---


## When you learn about zip()


Obviously our friend learned about python's built-in _zip()_ function and cannot
contain himself until he uses it. Have a look at this idiocy:

```python
def create_attributes(self, dim_meta, attributes):
    saved_attributes = [
        AttributeMeta(
            account=self.account,
            dimension_meta=dim_meta,
            order=order,
            type=value_type,
            required=False,
        )
        for order, (_, value_type) in enumerate(attributes)
    ]
    for attr, (name, _) in zip(saved_attributes, attributes):
        attr.name_json = I18nText(name)
        attr.name = name
        attr.name_slug = attr.get_unique_for_name()
    value_meta.set_internal_order(saved_attributes)

    AttributeMeta.upsert(saved_attributes)

    return saved_attributes
```

At this point I wonder if he's trolling us? The for iterations are killing me:
* first `for order, (_, value_type) in enumerate(attributes)`
* then `for attr, (name, _) in zip(saved_attributes, attributes):`

If you notice in one iteration is picking up the `value_type` and in the next one
the `name`. In the end he is zipping *attributes* with *attributes*!

This guy must be _certifiable_, his code is shortening my life more than my
cigarettes.



---


## Coding inceptions


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

![This is some serious gourmet shit](./res/serious-gourmet.gif){:width="250px" height="141px"}


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


---


## Support or Contact

If you reconize your little stinkers, please write me a short message
[here](http://dev/null) and you might restore my respect for you.
