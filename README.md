# dtscript -- reformat dtrace(1M) one-liners

This is a simple command-line tool for converting a DTrace one-liner into a more
readable script.  I use it after a one-liner becomes gnarly enough that it's
better to edit it as a script.  It's not complete, and surely has bugs.

You run it exactly like you'd run dtrace(1M).  For example:

    $ ./dtscript -q -n 'syscall::write:entry/execname == "ls"/{ self->f = 1; }' \
          -n 'syscall::write:return/self->f/{ @[ustack()] = count(); }' 
    #!/usr/sbin/dtrace -qs

    syscall::write:entry
    /execname == "ls"/
    {
    	self->f = 1;
    }

    syscall::write:return
    /self->f/
    {
    	@[ustack()] = count();
    }

