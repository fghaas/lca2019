# Limitations

<!-- Note -->
So, what are the things we **can't** do yet, or at least don't do very well?

Couple of things to mention here.


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

<!-- Note -->
Under some circumstances you may want to run VMs *within* a VM in a
lab environment, and in principle OpenStack can support that (or be
configured to). The problem there is that VMs with running nested VMs

* cannot be live-migrated, and
* cannot be resumed after being suspended.

(They just kernel-panic when waking up from managed save, or when
unpaused from live migration.)

We work around this limitation by force-rebooting VMs that use nested
virtualization.
