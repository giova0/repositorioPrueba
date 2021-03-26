Requirements
============

* Python 2.7 or Python 3.5+
* no external library requirements

Installation
============

You can install Queuelib either via the Python Package Index (PyPI) or from
source.

To install using pip::

    $ pip install queuelib

To install using easy_install::

    $ easy_install queuelib

If you have downloaded a source tarball you can install it by running the
following (as root)::

    # python setup.py install

FIFO/LIFO disk queues
=====================

Queuelib provides FIFO and LIFO queue implementations.

Here is an example usage of the FIFO queue::

    >>> from queuelib import FifoDiskQueue
    >>> q = FifoDiskQueue("queuefile")
    >>> q.push(b'a')
    >>> q.push(b'b')
    >>> q.push(b'c')
    >>> q.pop()
    b'a'
    >>> q.close()
    >>> q = FifoDiskQueue("queuefile")
    >>> q.pop()
    b'b'
    >>> q.pop()
    b'c'
    >>> q.pop()
    >>>

The LIFO queue is identical (API-wise), but importing ``LifoDiskQueue``
instead.

PriorityQueue
=============

A discrete-priority queue implemented by combining multiple FIFO/LIFO queues
(one per priority).

First, select the type of queue to be used per priority (FIFO or LIFO)::

    >>> from queuelib import FifoDiskQueue
    >>> qfactory = lambda priority: FifoDiskQueue('queue-dir-%s' % priority)

Then instantiate the Priority Queue with it::

    >>> from queuelib import PriorityQueue
    >>> pq = PriorityQueue(qfactory)

And use it::

    >>> pq.push(b'a', 3)
    >>> pq.push(b'b', 1)
    >>> pq.push(b'c', 2)
    >>> pq.push(b'd', 2)
    >>> pq.pop()
    b'b'
    >>> pq.pop()
    b'c'
    >>> pq.pop()
    b'd'
    >>> pq.pop()
    b'a'

RoundRobinQueue
===============

Has nearly the same interface and implementation as a Priority Queue except
that each element must be pushed with a (mandatory) key.  Popping from the
queue cycles through the keys "round robin".

Instantiate the Round Robin Queue similarly to the Priority Queue::

    >>> from queuelib import RoundRobinQueue
    >>> rr = RoundRobinQueue(qfactory)

And use it::

    >>> rr.push(b'a', '1')
    >>> rr.push(b'b', '1')
    >>> rr.push(b'c', '2')
    >>> rr.push(b'd', '2')
    >>> rr.pop()
    b'a'
    >>> rr.pop()
    b'c'
    >>> rr.pop()
    b'b'
    >>> rr.pop()
    b'd'


Mailing list
============

Use the `scrapy-users`_ mailing list for questions about Queuelib.

Bug tracker
===========

If you have any suggestions, bug reports or annoyances please report them to
our issue tracker at: http://github.com/scrapy/queuelib/issues/

Contributing
============

Development of Queuelib happens at GitHub: http://github.com/scrapy/queuelib

You are highly encouraged to participate in the development. If you don't like
GitHub (for some reason) you're welcome to send regular patches.

All changes require tests to be merged.

Tests
=====

Tests are located in `queuelib/tests` directory. They can be run using
`nosetests`_ with the following command::

    nosetests

The output should be something like the following::

    $ nosetests
    .............................................................................
    ----------------------------------------------------------------------
    Ran 77 tests in 0.145s

    OK

License
=======

This software is licensed under the BSD License. See the LICENSE file in the
top distribution directory for the full license text.

Versioning
==========

This software follows `Semantic Versioning`_

.. _Scrapy framework: http://scrapy.org
.. _scrapy-users: http://groups.google.com/group/scrapy-users
.. _Semantic Versioning: http://semver.org/
.. _nosetests: https://nose.readthedocs.org/en/latest/
