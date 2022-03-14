# Clear trash on Linux via the command line

Sometimes you might want to clear you user's trash and root's as well via the command line on Linux, here's how:

```bash
rm -rf /home/*/.local/share/Trash/*/** && sudo rm -rf /root/.local/share/Trash/*/**
```
