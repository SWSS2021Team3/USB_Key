# USB_Key
This repository is for USB key for Client, USB key for Server and all the other things.

## USB Key for Client
ClientKey/\*.\* should be copied to root path of USB, so rootca.crt file will be exist on G:\cert\rootca.crt. (Assume that USB flash drive is mounted on G:\)

	G:\>dir cert\
	 Volume in drive G is KWKIM_USB
	 Volume Serial Number is D9FD-7CCF

	 Directory of G:\cert

	2021-06-17  오전 07:44    <DIR>          .
	2021-06-17  오전 07:44    <DIR>          ..
	2021-06-16  오전 06:24             1,269 client.crt
	2021-06-16  오전 06:24             1,675 client.key
	2021-06-16  오전 06:24             1,294 rootca.crt
	               3 File(s)          4,238 bytes
	               2 Dir(s)   4,043,673,600 bytes free



## USB Key for Server
ServerKey/\*.\* should be copied to root path of USB, so rootca.crt file will be exist on USB:/cert/rootca.crt.
You must mount before copy the files.

mount command: 

    sudo mkdir /mnt/usb  
    sudo mount -t vfat /dev/sda1 /mnt/usb  
          
unmount command:  (if you need)
      
    sudo umount /dev/sda1

file position on USB:

	lg@LgFaceRecProject:~$ ls -l --file-type /mnt/usb/
	total 32
	drwxr-xr-x 2 root root 16384 Jun 16 04:12 cert/
	drwxr-xr-x 2 root root 16384 Jun 16 04:12 db/
	lg@LgFaceRecProject:~$ ls -l --file-type /mnt/usb/db/
	total 96
	-rwxr-xr-x 1 root root 17 Jun 16 04:12 facedb.cipherkey
	-rwxr-xr-x 1 root root 17 Jun 16 04:12 facedb.iv
	-rwxr-xr-x 1 root root 17 Jun 16 04:12 userdb.cipherkey
	-rwxr-xr-x 1 root root 17 Jun 16 04:12 userdb.iv
	-rwxr-xr-x 1 root root 17 Jun 16 04:12 videodb.cipherkey
	-rwxr-xr-x 1 root root 17 Jun 16 04:12 videodb.iv
	lg@LgFaceRecProject:~$ ls -l --file-type /mnt/usb/cert/
	total 144
	-rwxr-xr-x 1 root root 1269 Jun 16 04:12 facedb.crt
	-rwxr-xr-x 1 root root 1679 Jun 16 04:12 facedb.key
	-rwxr-xr-x 1 root root 1294 Jun 16 04:12 rootca.crt
	-rwxr-xr-x 1 root root 1269 Jun 16 04:12 server.crt
	-rwxr-xr-x 1 root root 1675 Jun 16 04:12 server.key
	-rwxr-xr-x 1 root root 1269 Jun 16 04:12 userdb.crt
	-rwxr-xr-x 1 root root 1675 Jun 16 04:12 userdb.key
	-rwxr-xr-x 1 root root 1269 Jun 16 04:12 videodb.crt
	-rwxr-xr-x 1 root root 1675 Jun 16 04:12 videodb.key

## All the other things
There are lots of another(or same) files in "AllKeyCert/".
All the **RSA Keys(private, public)**, **CSR files**, and **Certificate files** are here. To use these separately, you need to know a special OpenSSL command or use the OpenSSL API. We provide useful command at AllKeyCert/readme.txt file.
