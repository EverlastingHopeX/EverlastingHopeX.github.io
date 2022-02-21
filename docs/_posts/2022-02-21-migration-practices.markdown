Here I listed some practices I learned when roll out new feature while still support backwards compatibility.

# Practices

## Do not replace old field, add new field

We should not just put new format of data to an existing field without awareness of its impact (actually, never do this). By doing
this, we can keep the old data and rollback to older verision with no worry.

Only remove the old field when we can ensure that the new field can be handled perfectly (Hard to do in real scenario, but at least wait for 
a relatively long period of time)
