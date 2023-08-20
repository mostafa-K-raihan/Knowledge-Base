https://github.com/docker/for-win/issues/3171#issuecomment-554587817


for windows sometimes the dynamic port allocation done by the OS conflicts with the docker file's specified port, we need to set the range on higher numbers to get rid of the issue

`netsh int ipv4 show excludedportrange protocol=tcp`

`netstat -ano | findstr :8085`

`netstat -tuln`

To see current allocation.
`netsh int ipv[46] show dynamicport tcp`

To change the current allocation
`netsh int ipv[46] set dynamic tcp start=49152 num=16384`

Likely a restart is needed after that.