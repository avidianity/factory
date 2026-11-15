# Adoption plan template

Bootstrap shows this plan to the Owner before it writes anything.
For a new Project, leave out the sections about existing content.

````markdown
# Setup plan for <Project>

Kit version: <tag or commit>
Review mode: <Always | Critical only | Never>

## What I will add

- `<path>`: <purpose, in one line>

## What I will move or merge

- `<existing path>` goes to `<new path>`: <what happens to its content>

## Checks

| Check | Status |
| ----- | ------ |
| <check spec> | <new / already covered by <existing check> / not needed because <reason>> |

## Conflicts and your answers

| Topic | Your project does | The kit says | Your answer |
| ----- | ----------------- | ------------ | ----------- |
| <topic> | <current practice> | <kit rule> | <keep yours / take the kit's> |

## Labels I will create

<list>

## Open items

- <What I could not do, and why.>

**Do you approve this plan? Reply yes or no.**
````
