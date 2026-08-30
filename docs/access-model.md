# Access Model

## Fictional organisation

| User | Department | Groups |
| --- | --- | --- |
| Alice | Finance | All Employees, Finance |
| Bob | Engineering | All Employees, Engineering |
| Charlie | HR | All Employees, HR |

## Group-to-application mapping

| Group | Assignment | Result |
| --- | --- | --- |
| All Employees | Baseline workforce membership | Does not grant Finance Portal access in this lab |
| Finance | Finance Portal | Members receive the Finance Portal application assignment |
| Engineering | No Finance Portal assignment | Members do not receive Finance Portal through this group |
| HR | No Finance Portal assignment | Members do not receive Finance Portal through this group |

## Access decision

```text
Alice in Finance -> Finance group assigned to Finance Portal -> access allowed
Alice removed from Finance -> application assignment removed -> access denied
```

The lab uses group membership as the access-control point. This keeps application assignments aligned with department membership and makes revocation a single membership change rather than a direct user-by-user assignment update.
