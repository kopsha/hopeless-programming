---
layout: post
title: "Data Guys Cannot Write Code"
date: 2021-11-03
categories: hopeless-programming
---

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
> ![booleans](/res/booleans.jpeg){:width="300px" height="214px"}

