# Docker Internals

[https://www.youtube.com/watch?v=0Xb421-9CTo\&list=PLvctTrb5He\_TBqffs\_fDymhclgxdCIzR-\&index=2](https://www.youtube.com/watch?v=0Xb421-9CTo\&list=PLvctTrb5He\_TBqffs\_fDymhclgxdCIzR-\&index=2)

![](<../../.gitbook/assets/image (135).png>)

It all started here:

![https://en.wikipedia.org/wiki/Chroot](<../../.gitbook/assets/image (136).png>)

Then:

![](<../../.gitbook/assets/image (137).png>)

`systemd-cgls cpu`

`systemd-cgls memory`

`cat /proc/cgroups`

### **CGroups: Limits was processes use**

****[**https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt**](https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt)****

![](<../../.gitbook/assets/image (149).png>)

![](<../../.gitbook/assets/image (138).png>)

### Namespaces: limit was processes see:

![](<../../.gitbook/assets/image (140).png>)

![](<../../.gitbook/assets/image (150).png>)

![](<../../.gitbook/assets/image (139).png>)

![](<../../.gitbook/assets/image (142).png>)

![](<../../.gitbook/assets/image (143).png>)

![](<../../.gitbook/assets/image (147).png>)

![](<../../.gitbook/assets/image (151).png>)

![](<../../.gitbook/assets/image (152).png>)
