## Warning: Unmaintained.

This project is unmaintained. It was forked from [asavartzeth/docker-sftp](https://github.com/AsavarTzeth/docker-sftp) but receives no further love from either him nor me.

_See **Configuration Options** bellow, regarding $SFTP_DATA_DIR details (defaults to /data/sftp)_

###Storing data in a host directory###

    docker run --name some-sftp -v /path/container-data/sftp:/data/sftp -P -d asavartzeth/sftp

This is using the same principle as above. But instead of mounting the volume of another container you will mount a host directory.

This is useful in the sense that it could minimize filesystem overhead and would allow you to safekeep your data in a more traditional way. Another thing to note is, if you wish to use zfs, or another filesystem with no Docker backend, this enables a good compromise.

A possible downside to this approach might be lesser portability of your data.

##Exposing files & directories to the instance##

    docker run --name some-sftp -v /path/dir:$SFTP_DATA_DIR/chroot/share/dir -P -d asavartzeth/sftp

Preferably you would do this when you first deploy the container. However, you could certainly do the deployment as instructed under **"Deploying a simple sftp instance"**, commit and then re-deploy.

##Complex configuration##

For information on the syntax of the openssh configuration files, see the official [documentation](http://openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man5/sshd_config.5?query=sshd_config&sec=5).

If you wish to adapt the default configuration, use something like the following to copy it from a running container:

    docker cp some-sftp:/etc/ssh/sshd_config /some/path

You can use the modified configuration with:

    docker run --name some-sftp -v /some/path:/etc/ssh:ro -P -d asavartzeth/sftp

#Configuration Options#

This is a full list of environment variables that will be used in the configuration of your instance.

- -e `SFTP_USER=...` (defaults to sftp1)
- -e `SFTP_UID=...` (defaults to 2001)
- -e `SFTP_PASS=...` (defaults to randomly generated password)
- -e `SFTP_LOG_LEVEL=...` (defaults to INFO)  
The possible values are: QUIET, FATAL, ERROR, INFO, VERBOSE, DEBUG, DEBUG1, DEBUG2, and DEBUG3.
- -e `SFTP_DATA_DIR=...` (defaults to /data/sftp)  
*This will set the location of a data volume. It is used by the runtime script, to enable the transfer of application data to an empty location. This location could be a data volume container, such as [tianon/true](https://registry.hub.docker.com/u/tianon/true/), or a location on your host.*
