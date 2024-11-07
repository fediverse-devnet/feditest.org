* Go to the [UBOS Developer setup](https://ubos.net/docs/gears/developer/setup/), pick
  one of the options, and follow the instructions.

* Come back here once you have your development virtual machine running, and the Linux
  container in it.

* Now, in the non-root shell you started in your container with `machinectl shell ...`:

* Get FediTest (it's in the UBOS package repos):

  ```
  $ sudo pacman -S feditest
  ```

* Get the tests that test the Fediverse (for readability, shown on several lines):

  ```
  $ git clone --recurse-submodules \
    https://github.com/fediverse-devnet/feditest-tests-fediverse.git
  $ cd feditest-tests-fediverse
  $ git checkout v$(feditest version)
  ```
