Signal Meter
============

Objective: develop a way to visualize some signal strength data for a cellular
network.

This designed to be a short programming project -- please spend no more than
four hours on it. It's also designed to be a much larger problem than can
possibly be adequately solved in that timeframe -- so prioritize what you will
and will not implement, and we'll use that as a starting point to discuss your
implementation and design decisions. Quality over quantity.

The Problem
-----------

Imagine you're a running one of our rural cellular networks and want to
determine where there are dead spots in coverage in your town. Let's say that
we're able to gather location data from phones, as well as their signal
strength. The phones report this data periodically. Unfortunately, you don't
control the location of the phones, and sometimes they turn off. Phones can
also be poorly behaved: sometimes they report inaccurate location data. Radio
signals are complicated: signal strength can vary significantly in both space
and time, and propagation can be affected by exogenous factors that aren't
really intuitive.

Your goal: build an interface to help understand what real-world coverage looks
like for your network.

There are lots of complications in this problem, such as:
- How do you handle nonsensical behavior from phones?
- What information is most relevant to the operator?
- How should that information be presented?
- How should you handle adding multiple base stations (BTS) to the network?
- How do you store and process the stream of events?
- How do you validate your model against real-world performance?

"Phones" and the world
----------------------

To simplify this problem, we're going to be in Grid World. Each phone is driven
by a trace file under /traces, which has the following format:

    # offset, pos_x, pos_y, rssi
    0,10,10,-90
    60,10,10,-85

- offset: Time in seconds from start of simulation to send the report
- pos_x: The x position of the phone
- pos_y: The y position of the phone
- rssi:  Received signal strength indication, in dBm

We assume the BTS is located at 0,0. You are free to attach other meanings to
these numbers if you want (e.g., you can convert these values to a lat/long if
that's helpful for you).

The phone simulator is implemented in phone.py. It can be configured in
config.yaml. Once run, it will send a request to the specified URL containing
its report as a JSON string (for testing purposes, you can also have it output
to stdout). It currently contains minimal error checking, so you may want to
modify it to suit what you're building.

The report is a JSON dictionary containing the following relevant fields:

- imsi: A unique identifier for the phone (generated randomly each time a Phone
      object is created)
- pos_x: The x position of the phone
- pos_y: The y position of the phone
- rssi: Received signal strength, in dBm

To run a single phone, you can run `phone.py traces/phone0.trace`. To run a
phone for each trace in /traces, just run `sim.bash`.

Deliverables
------------
At minimum, you should implement a server that can process the JSON reports
from the phones. It must at least accept well-formed requests and send an
appropriate error to malformed ones. It must also be able to display the
reports that it's received in some way. We strongly prefer your implementation
be written in Python, as that's the primary language we use at Endaga.

We are interested in seeing how you approach a problem and think about user
experience, so we encourage you to design beyond this minimum requirement, even
if you aren't able to implement it all.

Feedback on this problem (implementation, documentation, etc) is also welcomed
and encouraged!
