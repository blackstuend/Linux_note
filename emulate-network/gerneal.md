# Cisco basic instrument


* Add enable mode password (Plain text)

```
R1(con-if) enable password cisco
```

* Add enable mode password (Cipher text)

```
R1(con-if) enable secret cisco
```

## Use Telnet's way to connect Switch

* Set Telnet password
```
R1(config)# line vty 0 4
R1(config-line)# password 123456
R1(config-line)# login
R1(config-line)# transport input telnet
R1(config)# service password-encryption //add Cipher text
```

## Add safe timeout 

```
# R1(config) line console 0 
# R1(config-line)# exec-timeout 0 5
```

## Save logging

```
# R1(config-line) logging synchronous
```

## 防止跳出控制台訊息


```
# R1(config) no ip domain-lookup
```