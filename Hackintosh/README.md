# 无限重启解决

**Error Info**
```
panic(cpu 0 caller xxxxxxxxxxxxx): initproc exited --exit reason namespace 2 subcode 0x4 description: none debugger caller: <panic>
```
**Solution**
***Add code below to the end of .vmx config file***
```
smc.version = "0"
cpuid.1.eax = "00000000000000010000011010100101"
```
