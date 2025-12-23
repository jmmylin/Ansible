# What's going on in this lab.
I am configuring EIGRP on the routers.

## Things I learned.
- I can have multiple runs in a playbook.
- before: <-- Will run this first if there needs to be a change. Notice it wipes out the whole EIGRP process.
- match: line <-- Will check the existing config (idempotency) playbook won't reapply unchanged lines.

## Issues
- Some routers had partial configs because Ansible might have been too quick. I ended up removing the before: line.
- The basic config.ios.ios does not really remove extra lines. That is why it was blowing away the existing eigrp 100 and then re-adding it.