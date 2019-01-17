# Limitations

<!-- Note -->
So, what are the things we **can't** do yet, or at least don't do very well?


## No “Save game” support

<!-- Note -->
First we‘d love the opportunity to have savepoints. This would mean
giving the learner the option to take a snapshot of their current lab
environment, then run a tricky lab or a potentially destructive
experiment, and if that fails, revert back to a known good state.

And OpenStack Heat even does support such a feature (it‘s called stack
snapshot and rollback there), but unfortunately it isn‘t fully baked
yet — it does not play nicely with nested stack resources, for example.


## Nested virtualization
