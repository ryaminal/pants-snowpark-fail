Having an issue with Pants and `snowflake-snowpark-python` library.

attempt to do 
```bash
pants generate-lockfiles --resolve=python-default
````

The corresponding error is something like:
```
19:09:43.13 [ERROR] 1 Exception encountered:

Engine traceback:
  in `generate-lockfiles` goal

ProcessExecutionFailure: Process 'Generate lockfile for python-default' failed with exit code 1.
stdout:

stderr:
pid 75241 -> /Users/ryan.despain/.cache/pants/named_caches/pex_root/venvs/9cab1b09d92c35cb0fcb3ac5f7a2c200b8d7bd3e/a94e5c7888694eb2dbf0d3c7ec2c78ae3a95e2e0/bin/python -sE /Users/ryan.despain/.cache/pants/named_caches/pex_root/venvs/9cab1b09d92c35cb0fcb3ac5f7a2c200b8d7bd3e/a94e5c7888694eb2dbf0d3c7ec2c78ae3a95e2e0/pex --disable-pip-version-check --no-python-version-warning --exists-action a --no-input --isolated -q --cache-dir /Users/ryan.despain/.cache/pants/named_caches/pex_root/pip_cache --log /private/var/folders/s2/fjg_n9md6g1gchtn40bt1vzw0000gp/T/pants-sandbox-VyMhz1/.tmp/pex-pip-log.sf_hs4nn/pip.log download --dest /private/var/folders/s2/fjg_n9md6g1gchtn40bt1vzw0000gp/T/pants-sandbox-VyMhz1/.tmp/tmpmpl29m3z/Users.ryan.despain..pyenv.versions.3.10.12.bin.python3.10 snowflake-connector-python snowflake-snowpark-python --index-url https://pypi.org/simple/ --retries 5 --timeout 15 exited with 1 and STDERR:
ERROR: Could not find a version that satisfies the requirement snowflake-snowpark-python
ERROR: No matching distribution found for snowflake-snowpark-python
```

Which is odd because that library does exist. I've tried a few things. Take the command from the `stderr` above and run it:
`/Users/ryan.despain/.cache/pants/named_caches/pex_root/venvs/9cab1b09d92c35cb0fcb3ac5f7a2c200b8d7bd3e/a94e5c7888694eb2dbf0d3c7ec2c78ae3a95e2e0/bin/python -sE /Users/ryan.despain/.cache/pants/named_caches/pex_root/venvs/9cab1b09d92c35cb0fcb3ac5f7a2c200b8d7bd3e/a94e5c7888694eb2dbf0d3c7ec2c78ae3a95e2e0/pex --disable-pip-version-check --no-python-version-warning --exists-action a --no-input --isolated -q --cache-dir /Users/ryan.despain/.cache/pants/named_caches/pex_root/pip_cache --log /private/var/folders/s2/fjg_n9md6g1gchtn40bt1vzw0000gp/T/pants-sandbox-VyMhz1/.tmp/pex-pip-log.sf_hs4nn/pip.log download --dest /private/var/folders/s2/fjg_n9md6g1gchtn40bt1vzw0000gp/T/pants-sandbox-VyMhz1/.tmp/tmpmpl29m3z/Users.ryan.despain..pyenv.versions.3.10.12.bin.python3.10 snowflake-connector-python snowflake-snowpark-python --index-url https://pypi.org/simple/ --retries 5 --timeout 15`
That command seems to install and work just fine.

Another variant is to change the `3rdparty/python/requirements.txt` to have just one thing, like `rich` or something. Then generate lockfiles, then pants export, then `activate` that exported env.
When that is done, then try a `pip install snowflake-snowpark-python` and that works, as expected.

Not certain what I'm missing here. `snowflake-snowpark-python` is the first/only lib i've encountered that has a problem.
