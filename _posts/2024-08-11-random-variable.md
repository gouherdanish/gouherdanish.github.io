

#### Implementation
```
from random_event import RandomEvent
from random_variable import RandomVariable

events = [
        RandomEvent(0,0.125),
        RandomEvent(1,0.375),
        RandomEvent(2,0.375),
        RandomEvent(3,0.125),
    ]
X = RandomVariable(events)  # X = {0: 0.125, 1: 0.375, 2: 0.375, 3: 0.125}
print(X.information())      # I(X) = [2.07944154 0.98082925 0.98082925 2.07944154]
```