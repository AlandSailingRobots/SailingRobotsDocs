POWERTRACKNODE DOC
==================

The purpose of the powertracknode is to track the power of both the solar power and the power that the system is using (excluding the actuators). This makes it convinient to track the power usage of the boat while it is testing as the details of how ASPire behaves in terms of its power balance is unknown.

The lastest and current state of the node as of 31/08/2018 is in the experimental branch. Where a test sail has shown that it was only sending default values, despite the fact that it had been working on another test setup before the sail. It is suspected that the different hardware connection from the test unit and the actual navigation may slightly differ, which causes the default values to appear.

Further work that can be made on this node would be to implement the power usage from the actuators and take into account into the entire power balance calculation. There is also a possibility to implement a QoL feature, which could give a warning when an Actuator is taking too much power. This safety feature can help reduce waste of power and making sure that the actuators do not short circuit.

The powertracknode has three files.

1. PowerTrackNode.cpp
2. PowerTrackNode.h
3. PowerTrackMsg.h

### PowerTrackNode.cpp & PowerTrackNode.h

These files have included a message subscription from the CurrentSensorData. It has details regarding the current, voltage, and the source of the data from either the solar panel or the system itself. The actuator unit is already included, but it is not available for use to some problems.

The actual calculation of the power is by simply multiplying the current with voltage, and having a switch that would ensure that the data sent from the solar panel and the system are equal (as it would prevent data overflow). A power balance is recorded and it tracks the net power usage in the mission.

### PowerTrackMsg.h

This file is used for both files above, but it is also for sending data into the database. Integration with the sailing website is not complete. However, the data sent can still be accessed in the terminal.
