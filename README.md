# Derper
A janky Docker replacement for spinning up a ton of temporary Meteor instances for testing.

## Commands

- **`config`**: Allows getting, setting, and removing configs from the `derperrc`.
  - `remote [|add|remove]`
    
    See the current remote, or add one `derper config add remote GHUser/GHRepo` and remove it.
  - `url [|add|remove]`
    
    See the current URL to which your temps will be deployed via nginx. Or add/remove, as above.
- **`serve`**: Serve a microserver.
  - `derper serve master`
    
    Serve the master branch at `master.[url]`, where `url` is specified in the config.
- **`cleanup`** Kills and cleans up after all temps.
- **`broadcast`** Broadcast a command to all of the meteor directories. Useful for things like `derper broadcast "rm annoyingfile"`


## Example session

```
derper config remote add j6k4m8/meteor-todo
derper config url add todos.jordan.matelsky.com

derper serve testbranch1

derper roll
```

This will result in the `testbranch1` branch of the `meteor-todo` repo being served at `testbranch1.todos.jordan.matelsky.com`, and `testbranch2` being served at `testbranch2.todos.jordan.matelsky.com`.

This makes deploying microservers very easy:

```python
from subprocess import call
for branch in ['master', 'dev', 'production', 'janky-fix']:
    call(['derper', 'serve', branch])
call(['derper', 'roll'])
```

The trailing `derper roll` is very important: This is what gives the signal for nginx to roll the new configuration.


<img src="https://cloud.githubusercontent.com/assets/693511/12869381/c9085f7a-cceb-11e5-9058-3863764413dd.png" width="50%">

