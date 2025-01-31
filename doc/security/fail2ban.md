# Protecting code-server from Bruteforce Attempts

<!-- TODO: remove this notice -->
### **NOTE: FAILED LOGIN ATTEMPT LOGGING IS NOT IN THE CURRENT VERSION AND WILL BE RELEASED IN V2.**

code-server outputs all failed login attempts, along with the IP address,
provided password, user agent and timestamp by default. When using a reverse
proxy such as Nginx or Apache, the remote address may appear to be `127.0.0.1`
or a similar address unless the `--trust-proxy` argument is provided to
code-server.

When used with the `--trust-proxy` argument, code-server will use the last IP in
`X-Forwarded-For` (if provided) instead of the remote socket address. Ensure
that you are setting this value in your reverse proxy:

Nginx:
```
location / {
	...
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	...
}
```

Apache:
```
<VirtualEnv>
	...
	SetEnvIf X-Forwarded-For "^.*\..*\..*\..*" forwarded
	...
</VirtualEnv>
```

It is extremely important that if you enable `--trust-proxy` you ensure your
code-server instance is not accessible from the internet (block it in your
firewall).

## Fail2Ban

Fail2Ban allows for automatically banning and logging repeated failed
authentication attempts for many applications through regex filters. A working
filter for code-server can be found in `./code-server.fail2ban.conf`. Once this
is installed and configured correctly, repeated failed login attempts should
automatically be banned from connecting to your server.
