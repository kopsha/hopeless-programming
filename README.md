## Welcome to hopeless programming


Nobody cares for good (or even best) practices, so it is time to expose them jewels here.
Enjoy these little stinkers I had to work with.


### I can do anything syndrome

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
Can you feel the _brain fart_ of this poor little fucker? He clearly has some discipline,
trying to keep the things readable, but obviously he has no clue how to do it. But that
does not stop him, he can overpower anything with his brain... is it?

He has not heard of higher level concepts. This guy codes like he's working out.


### Terrible idea two

This little stinker I found in a django project, with rest framework and all that jazz.
But that's too mainstream for my dude, he prefers to serialize the values on his own terms.

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

_Rule of thumb_: Never trust an `if`, I hear they are highly criticized lately. Use `assert` instead.


### Tell me you're an amateur, without ...

This guy had no friends in the coding business. I say "no friends", but *no acqauintances*
would be more precise; no one in his circle to talk about his misery...

I share this sh\*tty piece with you untouched, because I want you to know this is for
real and I am afraid his madness is contagious. So *Ctrl-C + Ctrl-V*:

```python
class DimensionCreatedOnDevice(BaseModel):
    """
    Marks dimensions that were added to a DDM of a given trial because they were either:
    - created on the mobile app
    - linked to a new plot on the mobile app, and the dimension wasn't present in a server DDM at the time of sync
      (e.g. the user selected the dimension from the trial's customer_dimensions list)

    Note that there is no data migration:
    only records created after this model will be marked.

    Currently this is used only by trials with a manual layout,
    and it was introduced together with that layout (hence why the data migration is missing).
    """

    trial = models.ForeignKey("Trial", on_delete=models.CASCADE)
    dimension = models.ForeignKey("Dimension", on_delete=models.CASCADE)

    class Meta(BaseModel.Meta):
        unique_together = ("trial", "dimension")
```

The comment says it clearly: this is not an accident, nor an experiment. You may notice
the clean coding discipline: clear and simple names, using whitespace in code and comments,
finally some comments that mention **why** the code is there (woohoo!). So this is not
happening in his first year, but clearly his reached his limits...

I have only one word for you mate:

![booleans](./res/booleans.png)




### zipping things just to ignore them

Have a look at this idiocy:

```python
def create_attributes(self, dim_meta, attributes):
    saved_attributes = [
        AttributeMeta(
            tenant=self.tenant,
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

At this point i believe he's just trolling us, the for iterations are killing me:
* first `for order, (_, value_type) in enumerate(attributes)`
* then `for attr, (name, _) in zip(saved_attributes, attributes):`

If you notice in one iteration is picking up the `value_type` and in the next one
the `name`. In the end he is zipping *attributes* with *attributes*!

This guy must be _certifiable_, his code is shortening my life more than my cigarettes.


### Support or Contact

If you reconize your little stinkers, please write me a short message [here](http://dev/null) and I might restore my respect for you.
