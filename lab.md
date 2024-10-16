# Scenario:
Our state machines are modeled from scenario 1 (parallel tracks do not cross, car road intersects both tracks)

# Invariants:
1. An approach signal will always precede a depart signal
1. Trains on each track only run in one direction.
1. The power will never be interrupted
1. Depart signal will never arrive before the elapsed signal
1. Barrier will always be closed when train is going by
1. A second train on the same track will not hit the approach sensor before the first train hits the depart sensor
1. Alarm is ringing 10 seconds before a train arrives, when the train is present, and 10 seconds after a train departs
1. The barrier will always be raised when no train is present


# Varying Invariants

Invariants that the example FSM assumes:
1. Only one train total (north or south) is present between the two sensors during anytime the alarm is ringing

Counter example sequence that makes the invariant false:
1. northbound train approaches
1. system enters state "ringing, arms up"
1. Timer elapses, system enters "ringing, arms down"
1. Southbound train approaches while in the "ringing, arms down" state
1. Northbound train departs, system enters state "ringing, arms up"
1. Southbound train goes through the crossing while gates are up, safety hazard, invariant is false


# Prove it.

| number | arms_down | alarm_on | northbound_present | southbound_present | north_approach | south_approach | north_depart | south_depart | time_elapsed | safety_hazard | next state |
|--------|-----------|----------|--------------------|--------------------|----------------|----------------|--------------|--------------|--------------|---------------|------------|
| 0      | 0         | 0        | 0                  | 0                  |0               |0               |0             |0             |1             |               |0,5,6,7     |
| 1      | 0         | 0        | 0                  | 1                  |0               |1               |0             |0             |0             | 7             |5           |
| 2      | 0         | 0        | 1                  | 0                  |1               |0               |0             |0             |0             |7              |6           |
| 3      | 0         | 0        | 1                  | 1                  |1               |1               |0             |0             |0             |7              |7           |
| 4      | 0         | 1        | 0                  | 0                  |0               |0               |1             |1             |0             |               |0,5,6,7     |
| 5      | 0         | 1        | 0                  | 1                  |0               |0               |0             |0             |0             |               |13,7        |
| 6      | 0         | 1        | 1                  | 0                  |0               |0               |0             |0             |0             |               |14,7        |
| 7      | 0         | 1        | 1                  | 1                  |0               |0               |0             |0             |0             |               |15          |
| 8      | 1         | 0        | 0                  | 0                  |0               |0               |0             |0             |0             |8              |0           |
| 9      | 1         | 0        | 0                  | 1                  |0               |0               |0             |0             |0             |7              |13          |
| 10     | 1         | 0        | 1                  | 0                  |0               |0               |0             |0             |0             |7              |14          |
| 11     | 1         | 0        | 1                  | 1                  |0               |0               |0             |0             |0             |7              |15          |
| 12     | 1         | 1        | 0                  | 0                  |0               |0               |0             |0             |0             |8              |4           |
| 13     | 1         | 1        | 0                  | 1                  |0               |0               |0             |0             |1             |               |4,14,15     |
| 14     | 1         | 1        | 1                  | 0                  |0               |0               |0             |0             |1             |               |4,13,15     |
| 15     | 1         | 1        | 1                  | 1                  |0               |0               |0             |0             |1             |               |4,13,14     |

| number | invariant |
|--------|-----------|
| 16     | 1         |
| 17     | 2         |
| 18     | 3         |
| 19     | 4         |
| 20     | 5         |
| 21     | 6         |
| 22     | 7         |
| 22     | 8         |

See labFSM.pdf for the result of this table.