## wait-for-it

`wait-for-it.sh` is a pure bash script that will wait on the availability of a host and TCP port.  It is useful for synchronizing the spin-up of interdependent services, such as linked docker containers.  Since it is a pure bash script, it does not have any external dependencies.

`wait-for-it.sh` 是一个纯粹的 bash 脚本，它将等待主机和 TCP 端口的可用性。它可以用于同步相互依赖的服务的启动，例如编排 docker 容器的启动顺序，由于它是纯粹的 bash 脚本，因此它不需要任何的外部依赖项。

## Usage

```
wait-for-it.sh host:port [-s] [-t timeout] [-- command args]
-h HOST | --host=HOST       Host or IP under test
-p PORT | --port=PORT       TCP port under test
                            Alternatively, you specify the host and port as host:port
-s | --strict               Only execute subcommand if the test succeeds
-q | --quiet                Don't output any status messages
-t TIMEOUT | --timeout=TIMEOUT
                            Timeout in seconds, zero for no timeout
-- COMMAND ARGS             Execute command with args after the test finishes
```

```
wait-for-it.sh host:port [-s] [-t timeout] [-- command args]
-h HOST | --host=HOST       测试服务的 HOST 或 IP
-p PORT | --port=PORT       测试服务的 TCP 端口
                            或者，你可以将主机和端口直接指定为 host：port 的形式
-s | --strict               指定后，只有测试服务测试成功的情况下才会执行子命令
-q | --quiet                不要输出任何状态消息
-t TIMEOUT | --timeout=TIMEOUT
                            超时（以秒为单位），0 表示无超时
-- COMMAND ARGS             测试完成后，使用 args 执行命令
```

## Examples

For example, let's test to see if we can access port 80 on www.google.com, and if it is available, echo the message `google is up`.

```
$ ./wait-for-it.sh www.google.com:80 -- echo "google is up"
wait-for-it.sh: waiting 15 seconds for www.google.com:80
wait-for-it.sh: www.google.com:80 is available after 0 seconds
google is up
```

例如，让我们测试一下我们是否可以访问 www.baidu.com 上的端口 80，如果可用，回显“百度状态可用”的消息。
```
$ ./wait-for-it.sh www.baidu.com:80 -- echo "百度状态可用"
wait-for-it.sh: waiting 15 seconds for www.baidu.com:80
wait-for-it.sh: www.baidu.com:80 is available after 0 seconds
百度状态可用
```

You can set your own timeout with the `-t` or `--timeout=` option.  Setting the timeout value to 0 will disable the timeout:

```
$ ./wait-for-it.sh -t 0 www.google.com:80 -- echo "google is up"
wait-for-it.sh: waiting for www.google.com:80 without a timeout
wait-for-it.sh: www.google.com:80 is available after 0 seconds
google is up
```

The subcommand will be executed regardless if the service is up or not.  If you wish to execute the subcommand only if the service is up, add the `--strict` argument. In this example, we will test port 81 on www.google.com which will fail:

```
$ ./wait-for-it.sh www.google.com:81 --timeout=1 --strict -- echo "google is up"
wait-for-it.sh: waiting 1 seconds for www.google.com:81
wait-for-it.sh: timeout occurred after waiting 1 seconds for www.google.com:81
wait-for-it.sh: strict mode, refusing to execute subprocess
```

If you don't want to execute a subcommand, leave off the `--` argument.  This way, you can test the exit condition of `wait-for-it.sh` in your own scripts, and determine how to proceed:

```
$ ./wait-for-it.sh www.google.com:80
wait-for-it.sh: waiting 15 seconds for www.google.com:80
wait-for-it.sh: www.google.com:80 is available after 0 seconds
$ echo $?
0
$ ./wait-for-it.sh www.google.com:81
wait-for-it.sh: waiting 15 seconds for www.google.com:81
wait-for-it.sh: timeout occurred after waiting 15 seconds for www.google.com:81
$ echo $?
124
```

## Community

*Debian*: There is a [Debian package](https://tracker.debian.org/pkg/wait-for-it).
