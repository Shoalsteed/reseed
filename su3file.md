SU3 files
=========

What is a SU3 file?
-------------------
An SU3 file (_s_igned _u_pdate) contains several objects. On the one hand, an SU3 file contains
another file of any format, metadata (date, version, ...) and a signature to
verify the file and the metadata. A detailed (technical) description can be
found at https://geti2p.net/spec/updates.

Previous versions
-----------------
Before the SU3 file format existed, the .sud and the .su2 file format already
existed, but these file formats had errors. These included that there was no
Magic Number and the signer (see below) was not given, so all known keys had
to be tried. Furthermore, a lot of metadata was still missing.

What exactly does an SU3 file contain?
--------------------------------------
As mentioned above, an SU3 file contains another file, a signature and metadata.
The signature is used to verify the author of the additional file. A public key
process is used for this purpose. This means that (as with GnuPG) there are two
keys, a public and a private one. By key is meant a digital file that makes it
possible to use certain cryptographic functions. The private key is used to sign
the file. This is comparable to an analog signature. However, to do this you
need a private digital key. The public key is used to verify the signature (or
analog signature), i.e. to check and confirm its authenticity. This is to confirm
that the file really comes from the author and not from an evil one. So this means
that you have to keep the private key secret. However, the public key can be made
publicly accessible. Since you also need this and to verify the signature. In the
case of a SU3 file, the person who signs (i.e. signs) the file is called the signer.
A signer is identified by an email address, which does not have to be real, but
finally specifies the format of an email address. In the SU3 format, this email
address is called SignerID. Summary: The SU3 file is signed (signed) with a
private key (a file). This signature (signature) can be verified (confirmed)
using the public key. This is done to confirm the authenticity of the sender of
a file. The person who signs is called Signer. The person's email address is
called SignerID, and (as mentioned above) metadata is also stored in an SU3 file,
which includes:
* The magic number. A magic number is always at the beginning of a file and gives
the type of the file (e.g. OpenDocument, PDF or SU3 file). With an SU3
The file is the magic number "I2Psu3".
* The type of signature. There are different types of keys and public key procedures.
The metadata therefore indicates which of these types is used.
* the SignerID (see above)
* The type of SU3 file (see below for the purpose). However, you can also save other files in a SU3 file.
* The type of the file. Supported types include:
    * ZIP files
    * XML files
    * HTML files
    * XML files compressed with GnuZip
    * Text (txt) files compressed with GnuZip
* The version. This is a number that indicates a date. Usually this is the date when the SU3 file was created.
* Different data so that the file is read correctly.

Usage
-----
There are several purposes for which an SU3 file is used:
* Unknown. This purpose is not further specified.
* Router*1 update. A SU3 file can contain a router update. Of course you could
also provide a router update in a "normal" simple ZIP file or something similar.
However, if you use an SU3 file, you can make sure that the update actually comes from the I2P team.
* Plugin or plugin update. There are various plugins for the I2P router. These
are generally intended to simplify the handling of I2P and various other applications.
Two known plugins are e.g. B. MuWire, a file-sharing program and I2P-Bote, a
decentralized email system. A plugin can either be delivered in an .xpi2p or a .su3
file. It is always recommended if you have the choice to use the SU3 file. Again
for the simple reason that this is how the authenticity of the developer can be
determined. Otherwise an evil person with such a plugin could severely damage the
router or the computer / server.
* Reseed hosts. So that the router can reconnect to the I2P network during the
first installation or after a long period of rest, it needs a few routers with
which it can connect at the beginning. However, these cannot come from other
routers, since his own router does not know anyone else, so a reseed file
containing router information is downloaded from the Clearnet or the Tor network.
So you need a file so that you can connect to the network after the first
installation of I2P. Taking an incorrect or manipulated file as a reseed file can
seriously endanger or even remove your own anonymity. So it is important that
such a reseed file comes from a trustworthy source. However, in order to check
the authenticity of this file, a SU3 file is required. For a reseed file, the SU3
file contains a ZIP file. Router information is stored in this ZIP file in order
to connect to the network.
* News (feed). These are the messages that always appear on the home page of the
router. A message can e.g. For example, a new I2P version may have been released.
Often the message file also contains a block list with dangerous IP addresses.
The router then does not connect to these. The news are saved in XML format in the SU3 file.
* Blocklist (feed). This function has not yet been implemented and is therefore
not yet documented. So there is unfortunately no explanation here.

*1 Here the router means the I2P router, i.e. the actual I2P software.

Future of the SU3 file
----------------------
_Warning: This section does not contain 100% verified information._
It doesn't seem that a new format will be released by the I2P team soon
becomes. So there will probably be no SU4 format. Except for the implementation
the blocklist feed function, this format is very mature.

Manually create a SU3 file with Java and the i2p.jar file
---------------------------------------------------------
_This section may be uninteresting or boring for non-developers or "normal"
users of I2P. Some prior knowledge will be required to understand this section._
To create a SU3 file you need the i2p.jar file. This can usually be found in the
I2P installation directory in the lib folder. For Linux ~/.i2p/lib/i2p.jar or
/usr/share/i2p/lib/i2p.jar. The instructions for this file will be lib/i2p.jar.
To be able to use the functions, the following Java command, which can be executed on the command line, is elementary:
    java -jar lib/i2p.jar su3file
It doesn't matter whether you write su3file in large or small letters.

### Generate key pair
To create an SU3 file, you first need a private and a public key. You can create this with the following command:
    java -jar lib/i2p.jar su3file keygen -t RSA_SHA512_4096 public.crt private.ks example@mail.i2p
The public key is saved in the file public.crt. The recipient also uses this file
to verify the SU3 file. The private key is stored in the file Private.ks. This
should never be passed on to anyone. The private key is stored in a JavaKeystore
file. example@mail.i2p is the SignerID here. These should be replaced accordingly
with his own replacement. Remember, this doesn't have to be a real email address,
it just needs to be in the format of an email address, and you'll be asked for a
password when you create it. This is used to secure the private key again. You
will need this password when signing a file with the private key. RSA_SHA512_4096
is the procedure used for the signature. The following are available:
	* DSA_SHA1
	* ECDSA_SHA256_P256
	* ECDSA_SHA384_P384
	* ECDSA_SHA512_P521
	* RSA_SHA256_2048	
	* RSA_SHA384_3072
	* RSA_SHA512_4096	
	* EdDSA_SHA512_Ed25519ph
If the -t argument is omitted, RSA_SHA512_4096 is used.

### Sign a file
The parameter sign is used for this:
    java -jar lib/i2p.jar su3file sign -c UNKNOWN -f HTML hello.html hello.su3 private.ks 1590434324 example@mail.i2p
-c describes the intended use. In this case, this is finally an insert
Example, therefore unknown. The following are available:
	* UNKNOWN
	* ROUTER
	* PLUGIN
	* RESEED
	* NEWS
	* BLOCKLIST
If none is specified, UNKNOWN is used. -f specifies the file format which contains
the SU3 file. (see What exactly does a SU3 file contain? Metadata section) The following are available:
	* ZIP
	* XML
	* HTML
	* XML_GZ
	* TXT_GZ
If none is specified, ZIP is used. Next, the file is specified, which should be
signed and packaged, in this example hello.html. Then the output file is specified.
In this case hello.su3. Then the file in which the private key is stored, next the
version number. In this case 1590434324. This is the typical UNIX submission format
for a point in time (https://en.wikipedia.org/wiki/Unix_time). Afterwards it is
specified with which SignerID the file should be signed. During the signing process,
a password is entered asked. This is the password that was entered when the key was created.

### Get file information
To do this, you can execute the following command:
    java -jar lib/i2p.jar su3file showversion hello.su3
Here is hello.su3, the file about which the information is to be obtained.
The following edition will appear:
Version:  1590434324 (25.05.2020, 21:18)
Signer:   example@mail.i2p
SigType:  RSA_SHA512_4096
Content:  UNKNOWN
FileType: HTML

### Verify signature
To verify the signature of a SU3 file, you need the file in which the public key is stored. You can then z. B. Execute the following command:
    java -jar lib/i2p.jar su3file verifysig -k public.crt hello.su3
Here is public.crt, the file in which the public key is stored and hello.su3 the SU3 file from which the signature is to be verified.

### Unpack content
You can do this with the following command:
    java -jar lib/i2p.jar su3file extract -x hello.su3
-x indicates that you do not want signature verification. If you want one, you
can use the parameter -k as with verify signature. hello.su3 is the file from
which the content is to be unpacked.


---
This document is available under the WTFPL license.
Copyright (c) 2000 Marek Küthe
This work is free. You can redistribute it and/or modify it under the
terms of the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar. See http://www.wtfpl.net/ for more details.
