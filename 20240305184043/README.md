# Permissions, groups and relationships fields

## Situation

There's an entity "A" with a Relationship field to an entity "B"

A group can edit entity "A" but has no access to entity "B" -> the 
dropdown of the Relationship field will be empty.

So the group needs edit permissions to "A" and access permissions to
"B". No need to give permissions to the fields.

**Unless** the Relationship field has a Filtering rule that uses one
of the fields of the entity "B". In this case, group needs Read Only
permissions to that field.


## Practical example

Group "Quality Assurance" can edit the entity "Input Lots" that has a
field named "Responsible" pointing to `sys.users`, filtered like this:

```
Values Group > Groups equals Quality Assurance
```

"Quality Assurance" needs "Access" permission to `sys.users` Entity
and "Read Inly" to the field "Group".
