#Chmod System Call

Documentation on the `chmod()` system call which is used to change the mode and/or permissions of a file. This system call involves changing permissions bits with either flags or octal integers.

##Inclusion

`#include <sys/types.h>`

`#include <sys/stat.h>`

##Declaration

`int chmod(const char *path, mode_t mode);`

##Return Value

If the file was changed successfully, then a 0 is returned. If an error occurs then a -1 is returned and the variable errno is set. 

[man page - chmod] (http://man7.org/linux/man-pages/man2/chmod.2.html)

[List of permissions](www.delorie.com/gnu/docs/glibc/libc_288.html)

##Examples

In the example below, we will see how `chmod()` can be used to change permissions.

```
#include <sys/stat.h>

int main(int argc, char **argv)
{
	char mode[] = "0777";
	char buf[100] = "/home/hello.t";
	
	int i;
	
	i = strtol(mode, 0, 8); //strtol converts to a long octal integer

	if (chmod (buf, i) < 0)
	{
		perror("chmod error");
		exit(1);
	}
	
	return 0;
}
```

In this example we are attempting to change the permissions of a file, which was named `hello.t`. WE can clearly see `chmod` passing in `buf` which has been initialized a specific path for `hello.t.` The variable `i` is then run with the `strtol` function which stores an octal integer. Both are then used with `chmod` with the path, and a new file permission to be passed through thus changing the permissions of the file. 

Normally we see more people using the actual flags, instead of the octal values. In the example below, we can see an instance of how permissions can be given with just the flag name, or multiple flag names.

```
#include <sys/stat.h> 

int main()
{
	const char *path = "/home/hello.t";
	
	int mod;
	
	mod = chmod(path, S_IRUSR | S_IRGRP | S_IROTH);
	
	if ( mod < 0)
	{
		perror("chmod");
		exit(1);
	}
	
	return 0;
}
```

As you can see, 3 different permissions are passed in. `S_IRUSR` is the read permission for the owner. `S_IRGRP` is the read 
permission for the group, and `S_IROTH` is the read permission for others. The permissions are split up by the `|` command, 
so thus any one of these permissions can be passed through giving permissions to the owner, group, and others.
If say for example, the `S_IRGRP` permission was removed, the above code would only give read permissions to the user and the
others. Starting to get how this works? 

Read permissions are not the only permissions you can give. Other flags can be passed in for different permissions. 

Within the same example, if instead of `S_IRUSR | S_IRGRP | S_IROTH` being passed in, if `S_IXUSR` is passed in, it will give execute and search permissions to the owner and not read permissions. So with the different flags, you can give read, write, execute/search permissions to the owner, group, or others depending on what you see fit or need to do.

So as you can see, `chmod()` provides to be very beneficial if permissions ever need to be given or taken away for any 
reason. 



