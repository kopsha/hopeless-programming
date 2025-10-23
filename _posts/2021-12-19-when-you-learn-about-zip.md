---
layout: post
title: "When You Learn About zip()"
date: 2021-12-19
---

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

