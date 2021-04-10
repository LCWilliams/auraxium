==============
Object Caching
==============

.. currentmodule:: auraxium

Most objects accessible through the PlanetSide 2 API are static, meaning that the data returned will remain the same between polls.

Auraxium uses a caching system to reduce API load by keeping objects in memory past their initial lifetime. The next time this item is accessed, it is instead restored from cache without incurring any network traffic.

This is done via a local `TLRU Cache`_ specific to each cacheable object type. This type of cache has two replacement mechanisms: The first acts like a stack, with items being moved to the top as they are (re)used, and the bottom ones being discarded as the cache reaches its size limit. This is also referred to as a "least recently used" (LRU) cache.

Additionally, cache items have a maximum lifetime to force updates for regularly used objects. This mostly ensures that somewhat-static objects are never too far out of date.

.. note::

    Restoring items from cache is currently only supported when using :meth:`Ps2Object.get_by_id`.

    For :class:`Named` sub classes, the :meth:`Named.get_by_name` method is similarly cached by locale. This means that looking up the same name in two different locales will create two separate cache entries.

Customising Caches
------------------

Each PlanetSide 2 datatype is preconfigured with a suitable cache size and item lifetime. Factions, for example, are only updated once an hour. Characters are limited to a size of 256 and only last 30 seconds before they are repolled.

You can override the default values at runtime using the :meth:`Cached.alter_cache` method:

.. automethod:: Cached.alter_cache
    :noindex:

.. _TLRU Cache: https://en.wikipedia.org/wiki/Cache_replacement_policies#Time_aware_least_recently_used_(TLRU)
