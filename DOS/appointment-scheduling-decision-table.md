| Conditions                     | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| ------------------------------ | --- | --- | --- | --- | --- | --- | --- | --- |
| Time slot available?           | Y   | Y   | Y   | N   | Y   | Y   | N   | Y   |
| Within working hours?          | Y   | Y   | N   | -   | Y   | Y   | -   | Y   |
| Patient record exists?         | Y   | N   | -   | -   | Y   | Y   | -   | Y   |
| Emergency case?                | N   | N   | -   | -   | Y   | N   | Y   | N   |
| Conflicting appointment?       | N   | N   | -   | -   | N   | Y   | -   | N   |
| Doctor available?              | Y   | Y   | -   | -   | Y   | Y   | -   | N   |
| Actions                        |     |     |     |     |     |     |     |     |
| Schedule appointment           | X   |     |     |     | X   |     |     |     |
| Create new patient record      |     | X   |     |     |     |     |     |     |
| Show error: Outside hours      |     |     | X   |     |     |     |     |     |
| Add to waitlist                |     |     |     | X   |     |     |     |     |
| Prioritize scheduling          |     |     |     |     | X   |     | X   |     |
| Suggest alternative time slots |     |     |     |     |     | X   |     | X   |
| Notify patient                 | X   | X   |     | X   | X   | X   | X   |     |
| Update doctor's schedule       | X   | X   |     |     | X   |     |     |     |

Legend:
Y = Yes, N = No, X = Action to be taken, - = Condition not relevant
