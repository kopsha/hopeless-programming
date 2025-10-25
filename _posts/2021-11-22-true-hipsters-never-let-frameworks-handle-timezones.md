---
title: "True Hipsters Never Let Frameworks Handle Timezones"
date: 2021-11-22
categories: hopeless-programming
---

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

