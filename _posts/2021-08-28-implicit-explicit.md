---
title: Explicit actions imply intent, prefer implicit actions
category: [programming]
---

Software development (as well as many things in life) is all about making
choices, one of them is deciding whether to do things in an implicit or
explicit way.

Generally (based on my personal experience) the explicit option is the one that
people recommend taking because it doesn't require knowledge of the implicit
behaviour and is more likely to be forward compatible.

For example, this could be when specifying arguments to a function, cli, etc.
specifying the same values as the default values even though not specifying
that argument at all would result in the same behaviour. Or in expressions,
adding brackets for precedence even though it is not needed:

```cpp
// Here the brackets are superfluous
bool perform_action = (timestamp == current_time) ? object.has_changed() : timestamp == 0;
```

However, I believe that expressing things implicitly would be better in many of
these cases because when expressing things explicitly an intent is implied:
it is implied there is a reason why the thing in question is defined
explicitly. "Why would someone go out of their way to do something?".

I believe that this implied intent can lead to confusion. For example it can
make someone believe the default is not what it is, or the operator precedence
is not in the order it actually is and as a result, the implicit option is
preferable in some if not most of these cases.

Additionally, the implicit option also saves time when developing.

If for some reason, the implicit way is not clear enough, documentation
(mainly through comments) I believe is a better solution than bearing this
implied intent and in the case of forward compatibility, often the new default
is what you want; if not, upgrading is usually trivial.

To conclude, in software development clarity is something that we want to
maximise, and I believe that more often than people think doing things
implicitly is the way to go about that.
