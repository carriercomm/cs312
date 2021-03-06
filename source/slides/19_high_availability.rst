.. _19_high_availability:

High Availability Strategies
============================


Notes
-----

* CoreOS image added to OpenStack

  * username is ``core``

* Questions for Brandon?

  * `Mailing List and IRC`_ (try these first!)
  * email Brandon at ``brandon@ifup.org``

* Homework 3 is being graded now
* Homework 4 due Wednesday
* Homework 5 assigned Wednesday

.. _Mailing List and IRC: https://coreos.com/community/

Terminology
-----------

* Redundancy
* Reliability
* Single point of failure (SPOF)
* Horizontal Scaling
* Vertical Scaling

Redundancy
----------

Often measured as the number of nodes that can fail before a failure scenario
occurs

Closely tied to reliability (more redundant systems usually have higher
reliability)

Reliability
-----------

The percentage of uptime vs total time.

* ideal is typically five 9s, ``99.999%``

  * This gives less than fifty three minutes of downtime per year

* A reasonably good goal is ``99.9%``

  * This allows for 100x more downtime than five 9s.

* Measuring uptime/downtime is **hard**

Downtime Measuring Example
--------------------------

Consider the following scenario:

You run an OpenStack cluster. One day, your authentication API goes down.

None of your customers can log in or interact with any of the other APIs.

All of the customers existing services continue running (VMs stay up, etc).

What is the downtime?

Scaling
-------

You can define scaling as adding more resources to increase performance,
reliability, or redundancy.

Two forms:

* Horizontal
* Vertical

Horizontal Scaling
------------------

Adding more nodes to a system.

Also known as scaling out.

Examples:

* Adding another web node
* Adding a second (or third, etc) database node

Horizontal Scaling
------------------

Pros:

  * Typically has higher upper bound than vertical scaling
  * Can bring greater increases than vertical scaling
  * Redundancy

Cons:

  * Expensive
  * Maybe not as much redundancy as you expect
  * Brings more complexity to manage
  * Unused capacity problems (pick: cost or even more complexity)

Horizontal Scaling Complexity
-----------------------------

Horizontal scaling increases complexity because:

* Requires load balancing, replication, etc
* Budgeting for peak load + X% can leave a lot of unused capacity
* Managing lots of nodes is harder than managing fewer nodes

Vertical Scaling
----------------

Adding more resources to a particular node(s)

Also known as scaling up.

Examples:

  * Adding a faster CPU
  * Adding more RAM
  * Adding faster/larger disks


Vertical Scaling
----------------

Pros:

  * Easier than horizontal scaling
  * No added complexity
  * Usually cheaper

Cons:

  * No redundancy (but maybe more reliable)
  * Has a lower upper bound
  * Diminishing returns

Scaling
-------

.. figure:: ../_static/scaling.png
   :align: center
   :width: 90%

Single Point of Failure
-----------------------

Traditionally a point with 0 redundancy, often instead means the point
in the system with the lowest redundancy value.

Examples:

  * Single load balancer with multiple web nodes
  * Single database node
  * Network switch
  * Non-redundant power

Single Point of Failure
-----------------------

Identifying SPOFs is a hard task.

Many places will do fire drills, where a system in staging/pre-production
is purposefully taken down so that failure scenarios can be observed, and single
points of failure can be identified and fixed.

You can read more about Netflix does this `here`_.

.. _here: http://techblog.netflix.com/2012/07/chaos-monkey-released-into-wild.html

Virtual IP
----------

* Doesn't correspond to a particular physical nic
* Shared between many nics across different machines (and one nic can have
  multiple addrs)
* Can be moved across any other ip on the same subnet
* Variety of implementations, ``carp`` and ``ucarp`` derived from OpenBSD

Virtual IP
----------

Limitations:

  * Doesn't handle the replication of data
  * Can't move across subnets
  * Really only good for making an IP address(es) redundant

