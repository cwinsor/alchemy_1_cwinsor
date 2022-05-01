This assumes you already have AWS Ubuntu host and
TightVNC  from laptop to the  Linux desktop
all set up and working.  If not see README_3.

## Every time:

### On laptop:
1. Review AWS IP address and .pem file and host status
2. Open 2 WSLs 
3. ssh -i pemfile.pem ubuntu@123.456.789.012 
* netstat | grep 5901 (to confirm vncserver running port 5901) 
4. ssh -i pemfile.pem -L 5901:120.0.0.1:5901 -C -N -l ubuntu 123.456.789.012
5. vncviewer to localhost:5901

