# exssh

Provide automation capability for any ssh client.

## Install

`pip install exssh`

with bitwarden cli support:

`pip install exssh[bitwarden]`


## Usage

- `CONNECT_CONFIG` connection config file (default: ~/.ssh/connect.conf)
- `EXPECT_CONFIG` expect config file (default: ~/.ssh/expect.conf)
- `ESCAPE` escape character to send (default: ^])
- `COMMAND_START` local command start for copy result (default: ``)
- `COMMAND_END` local command end for copy result (default: ^I)
- `PROG` ssh client program (default: ssh)
- `EXTRA` extra parameter for ssh client program

`python3 -m exssh [-h] [-p PORT] [--timeout TIMEOUT] [--connect-config CONNECT_CONFIG] [--expect-config EXPECT_CONFIG] [--escape ESCAPE] [--command-start COMMAND_START] [--command-end COMMAND_END] [--prog PROG] [--extra EXTRA] [--debug] host`

### CONNECT_CONFIG

``` config
[*]
prog=zssh
-z=^\\
extra=-t "tmux new-session -A -s main"

[*-vps]
prog=mosh
--predict=experimental
--predict-overwrite=
```

- config section title is matching ssh host of ~/.ssh/config
- prog and extra are the same functions of command arguments
- other configs are parameters specific to the choosing prog

### EXPECT_CONFIG

``` config
[ssh-host]
prompt = Welcome to HOST
user = Username
password = P@ssw0rd
```

- config section title is matching ssh host of ~/.ssh/config
- prompt is the indicator of successful connection in case not autodetected
- other keys are trigger keywords, values are auto filled responses

### Example

```bash
python3 -m exssh --connect-config ~/.ssh/connect.conf --expect-config ~/.ssh/expect.conf --escape \\x1b --command-start \!\! --command-end \& ssh-host
```

In this example, when connected to ssh-host:
press `<Esc>` to escape
type `!!cat ~/.bashrc&` will copy execute result into clipboard
