You can add it to the file `.profile` or your login shell profile file (located in your home directory).

To change the environmental variable "permanently" you'll need to consider at least these situations:

1.  Login/Non-login shell
2.  Interactive/Non-interactive shell

### bash

1.  Bash as login shell will load `/etc/profile`, `~/.bash_profile`, `~/.bash_login`, `~/.profile` in the order
2.  Bash as non-login interactive shell will load `~/.bashrc`
3.  Bash as non-login non-interactive shell will load the configuration specified in environment variable `$BASH_ENV`