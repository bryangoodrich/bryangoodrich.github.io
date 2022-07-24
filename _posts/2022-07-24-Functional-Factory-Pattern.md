---
layout: post
title: "Functional Factory Pattern"
date: 2022-07-24
excerpt: "Why FP does creation better"
tags: [design patterns, python, functional programming, code]
comments: true
---


# Functional Factory Pattern

## Background
The Factory Pattern, a creational design pattern in object-oriented programming (OOP), can obscure too much. Today I want to introduce a functional programming (FP) alternative perspective. 

For comparison, we can leverage the [Real Python - Factory Method Pattern](https://realpython.com/factory-method-python/) for inspiration and examples.

In the link above, they create a `Song` class and `SongSerializer` class (interface) for converting a `Song` object into a given serialized format. I leave it to the reader to delve into their lesson, as it is very good. Here is their final code.

```
class SongSerializer:
    def serialize(self, song, format):
        serializer = get_serializer(format)
        return serializer(song)


def get_serializer(format):
    if format == 'JSON':
        return _serialize_to_json
    elif format == 'XML':
        return _serialize_to_xml
    else:
        raise ValueError(format)


def _serialize_to_json(song):
    payload = {
        'id': song.song_id,
        'title': song.title,
        'artist': song.artist
    }
    return json.dumps(payload)


def _serialize_to_xml(song):
    song_element = et.Element('song', attrib={'id': song.song_id})
    title = et.SubElement(song_element, 'title')
    title.text = song.title
    artist = et.SubElement(song_element, 'artist')
    artist.text = song.artist
    return et.tostring(song_element, encoding='unicode')
```

They do note that the `.serialize()` method in the `SongSerializer` class does not use the `self` parameter. This concern should be ours as well, but not for the reason they give. 

My concern is this: just think about data and functions. All there is, or can be, are functions and data. 

Now before I dig into the refactoring of this example, I will preface that the benefit to the OOP approach is the tight binding of interfaces and objects to keep co-related code together with a common API. I just think an FP approach handles it better. That is for you to decide!

## What's in a Name?
Our number one concern as python developers should be to write pythonic code. If you are using factories to manage relating behavior (functions) to objects (data), I would say you are more than likely doing it wrong, if not for just depending on objects to store your data. 

Favor objects over primitives whenever they convey an advantage in use or reasoning.

With that said, consider the `Song` object.

```
class Song:
    def __init__(self, song_id, title, artist):
        self.song_id = song_id
        self.title = title
        self.artist = artist
```

This can be more easily defined using `dataclasses` or `namedtuple` if you want readability, with a lot of extra benefits to writing boilerplate code.

```
from dataclasses import dataclass

@dataclass
class Song:
    song_id: int
    title: str
    artist: str

song = Song(1, "Water of Love", "Dire Straights")
```

With `namedtuple`, if you want immutability by default. Did you freeze your class?? 

```
from collections import namedtuple

Song = namedtuple("Song", "song_id title artist")
song = Song(1, "Water of Love", "Dire Straights")
```

Even more concretely, just use standard `dict`.

```
song = {
    "song_id": 1,
    "title": "Water of Love",
    "artist": Dire Straits"
}
```

You might say, this last example is more unreadable and hard to work with, specifying it everytime. Then simply create a function to create your object-as-dict. In the end, every instantiation shown above is no more obscure than calling the object creation code.

```
def Song(id: int, title: str, artist: str) -> Song:
    return {
        "song_id": id,
        "title": title,
        "artist": artist
    }

song = Song(1, "Water of Love", "Dire Straights")
```

Now with this approach, we have song-as-dict and can instantiate that easily by calling a function to create them given some data. We can even add type annotation to name this type of dict as a `Song` type. 

## Functions. Functions Everywhere!
In the OOP approach serialization was left to a class instance that essentially wrapped a static function to dispatch calling another concrete implementation defined elsewhere. But if you're going to call other functions, what purpose does the class serve?

1. If you need a namespace, then explicitly collect them in a class as static functions and treat it as such, a way to call functions. Or
2. Put them into a module where you can import as-if it were such a class and call them in the same `Module.function` way.

The two points above do not address what the Factory Pattern tries to solve, however. The key is that it contains how you will *dispatch* a concrete implementation. There is a better way. 

Suppose you have your 2 functions defined. I would even recommend putting these into a common module, like `seralizers` where you can `import serializers as Serializers` and call them `Seralizers.to_json` or something to that affect.

```
def serialize_to_json(song):
    payload = {
        'id': song.song_id,
        'title': song.title,
        'artist': song.artist
    }
    return json.dumps(payload)


def serialize_to_xml(song):
    song_element = et.Element('song', attrib={'id': song.song_id})
    title = et.SubElement(song_element, 'title')
    title.text = song.title
    artist = et.SubElement(song_element, 'artist')
    artist.text = song.artist
    return et.tostring(song_element, encoding='unicode')
```

Now later the article demonstrates how to handle this through objects (ugh!) by making a `JsonSerializer` class and `XmlSerializer` class. This is exactly the anti-pattern I want you to avoid.

Instead of registering these functions as objects with a factory class that coordinates associating song (data) to behavior (function), we can do this much more directly, much less obscurely. 

```
def serialize(song: Song, serializer):
    return serializer(song)
```

That's it. 

That is really all the complication there is. How would it work?

```
serialize(song, serialize_to_json)
# {"id": "1", "title": "Water of Love", "artist": "Dire Straits"}

serialize(song, serialize_to_xml)
# <song id="1"><title>Water of Love</title><artist>Dire Straits</artist></song>

serialize(song, lambda s: yaml.dump(s))
# {artist: Dire Straits, id: '1', title: Water of Love}
```

## But Wait, There's More!

Now you may rightfully ask, where is the factory? You got me. I simply demonstrated above the *dispatching* to a function-as-parameter, because functions are data, too. The real beauty in functional programming is how to *organize* that data. 

The `SeralizerFactory` object in the OOP approach handled the factory method by registering your serializer classes with that factory instance. The way to do this in FP is to use *closures*. 

A closure simply *encloses* variable scope within a function by nesting a function inside a function. In other words, you create a function that returns a function. The dynamic part that mimicks the factory pattern is precisely that the inner function contains the variables that, well, vary! The outer function picks up the data that will be enclosed within, but available to, the inner function.

```
def outer(x):
    def inner(y):
        return x+y
    return inner

outer(5)(10)
# 15
```

The above example is to demonstrate the closure structure. How you use closures can solve a myriad of problems. 

It also demonstrates *partial application*. This is an idea called [currying](https://en.wikipedia.org/wiki/Currying).

Suppose in the example above we started with a function signature `add(x, y)`. By currying (partial application) we can *apply* one of the variables and hold it static. Thus every 2-parameter function is in essense a series of 1-parameter function calls. This is can be done using the `partial` functions from `functools`

```
from functools import partial

def add(y, x):
    return x+y

add5 = partial(add, 5)  # equivalent to outer(5)
add10 = partial(add, 10)  # equivalent to outer(10)

add5(25)  # or outer(5)(25)
# 30

add10(25)  # or outer(10)(25)
# 35
```

So what this functional programming method enables us to do is simplify the example same behavior the factory pattern did in a number of ways. Let me demonstrate both.

### Method 1 - Closures that hold our registered serializers

In this approach, we will register our functions and continue to dispatch in an according way, but without all the if-else nonsense. 

```
serializers = {
    "JSON": serialize_to_json,
    "XML": serialize_to_xml,
    "YAML": lambda s: yaml.dump(s)
}

def factory(serializers):
    def inner(song, format):
        return serializers.get(format)(song)
    return inner

serialize = factory(serializers)

serialize(song, "JSON")
# {"id": "1", "title": "Water of Love", "artist": "Dire Straits"}
```

### Method 2 -- Use partial application to abstract out part of your API

It is much easier to just think you have that dictionary of functions at your disposal. It may even be generated at runtime by some dynamic process that holds 100s of serializers you would not want to code by hand. Then you as a developer can just focus on the coding and let `functools` handle the abstraction. 

```
from functools import partial

serializers = {
    "JSON": serialize_to_json,
    "XML": serialize_to_xml,
    "YAML": lambda s: yaml.dump(s)
}

def serialize_factory(serializers, song, format):
    return serializers.get(format)(song)

serialize = partial(serialize_factory, serializers)

serialize(song, "JSON")
```

You may notice that the factory and the inner function *are identical implementations*. This is because they're basically doing the same thing. The use of `partial` does some extra metadata stuff, I'll leave [to the interested reader to discover](https://docs.python.org/3/library/functools.html). 

You tell me, which do you think is easier to reason about? Which is more expressive and direct about what it is trying to do? Which is easier to code up? If you, like me, think the FP approach is much more simple, I encourage you to discover more about FP languages and their patterns. 