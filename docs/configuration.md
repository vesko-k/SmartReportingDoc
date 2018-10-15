# Directory Structure

**&lt;InstallDir&gt;** - This is the installation folder. Here is
situated the starting executable of the service;

> **Config** - A folder containing configuration files of the
service;

> **Lib** - Common libraries. Contains a set of shared libraries
used by the service;

> **Logs** - Log files of the service;

# Service Configuration Files

Configuration of the service is located in **&lt;InstallDir&gt;\\Config\\**
folder:

**Smart.Reporting.PhdReportingServices.Configuration.xml** - Contains
configuration for the service;

# Configuring Service

The **WCF** service configuration is situated in  
"**&lt;InstallDir&gt;\\Config\\Smart.Reporting.PhdReportingServices.Configuration.xml**".

__Example:__

```xml
<?xml version="1.0" encoding="utf-16"?>
<Configuration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    ServiceUrl="net.tcp://localhost:29501/"
    SecurityMode="Transport"
    TcpTransportClientCredentialType="Certificate"
    HttpTransportClientCredentialType="None"
    MessageClientCredentialType="None"
    IncludeExceptionDetailInFaults="true"
    MaxBufferPoolSize="2147483647"
    MaxReceivedMessageSize="2147483647"
    MaxStringContentLength="2147483647"
    MaxDepth="2147483647"
    MaxArrayLength="2147483647"
    MaxBytesPerRead ="2147483647"
    MaxNameTableCharCount="2147483647"
    TempFolder="C:\Smart Phd Reporting Services\Upload"
    TempFolderFilesRententionTime="300"
    ServerCertificateHash="41C9655CE71838DD7ECCEB051E4BA9C388CB7F98"
    ValidateClientCertificateHashes="true"
    PermissionRole ="">

  <!-- If is set to "true". DefaultFirstChanceExceptionsHandler will be attached to the executing application domain. -->
  <!-- Uncomment this to enable debug traces on first chance exceptions. Use only for debugging purposes. -->
  <DebugTraceFirstChanceExceptions>true</DebugTraceFirstChanceExceptions>

  <!-- If is set to "true". DefaultUnhandledExceptionsHandler will be attached to the executing application domain. -->
  <!-- Uncomment this to enable debug traces on unhandled exceptions. Use only for debugging purposes. -->
  <DebugTraceUnhandledExceptions>true</DebugTraceUnhandledExceptions>

  <!--
  [SecurityMode]
        None      - Security is disabled.
        Transport - Security is provided using a secure transport (for example, HTTPS).
        Message   - Security is provided using SOAP message security.
        TransportWithMessageCredential - A secure transport (for example, HTTPS) provides integrity, confidentiality,
                                         and authentication while SOAP message security provides client authentication.
  [TcpTransportClientCredentialType]
        None        - Specifies anonymous authentication.
        Windows     - Specifies client authentication using Windows.
        Certificate - Specifies client authentication using a certificate.
        
  [HttpTransportClientCredentialType]  
        None              - Specifies anonymous authentication.
        Basic             - Specifies Basic authentication. For more information, see RFC 2617 – HTTP.
                            Authentication: Basic and Digest Authentication.
        Digest            - Specifies Digest authentication. For more information, see RFC 2617 – HTTP.
                            Authentication: Basic and Digest Authentication.
        Ntlm              - Specifies client authentication using NTLM.
        Windows           - Specifies client authentication using Windows.
        Certificate       - Specifies client authentication using a certificate.
        InheritedFromHost - The authentication is inherited from the host.
      
  [MessageClientCredentialType]
        None        - Specifies anonymous authentication.
        Windows     - Specifies client authentication using Windows.
        UserName    - Specifies client authentication using UserName.
        Certificate - Specifies client authentication using a certificate.
        IssuedToken - Specifies client authentication using an issued token.
        
  [TempFolderFilesRententionTime]
        In seconds.
  -->

  <PhdConnectionSettings Host="PHD-21L2-1"
                         Port="3150"
                         Username="mngr"
                         Password="manager"
                         ServerVersion="RAPI200"
                         WindowsUsername=""
                         WindowsPassword=""
                         MaxRows="0"
                         MinConfidence="100"
                         StringNotAvailable="#NA"
                         StringBadValue="#BAD"
                         StringError ="#ERR"
                         StringConnectionError="#CONN"
                         ConnectionTimeout="5000"
                         RetryPeriond="1000"/>

  <ClientCertificateHash>FD21799739FAB97740CF4C713D4455065C48206E</ClientCertificateHash>
  <ClientCertificateHash>41C9655CE71838DD7ECCEB051E4BA9C388CB7F98</ClientCertificateHash>
  <ClientCertificateHash>46803D2A3C156CE8C8B4A41624E75F1E6D925158</ClientCertificateHash>
</Configuration>
```

\[**ServiceUrl**\]

> The **ServiceUrl** describes scheme, **IP** address and port on which
the service should listen. The service can use **http**, **https** or
**net.tcp** binding.

> __Example:__

> `ServiceUrl="net.tcp://localhost:29501/`

> or

> `ServiceUrl="http://localhost:29501/"`

> An **URI** prefix string is composed of a scheme (**http,** **https** or
**net.tcp**), a host, an optional port, and an optional path. An example
of a complete prefix string is
<http://www.contoso.com:8080/customerData/>. Prefixes must end in
a forward slash ("**/**").

> *``Remark:`` For using **HTTPS** scheme or binding with transport
credential type **Certificate** you must provide a server certificate.
How to configure the **SSL** will be discussed later.*

> By default service listens on **HTTP** port **29501**
(<http://localhost:29501/PhdReportingServices>).

\[**SecurityMode**\]

> Defines transport protection level of the service.

> Available options are:

> - **Message** - Security is provided using **SOAP** message security;

> - **None** - Security is disabled;

> - **Transport** - Security is provided using a secure transport (for 
example, **HTTPS**);

> - **TransportWithMessageCredential** - A secure transport (for
example, **HTTPS**) provides integrity, confidentiality, and
authentication while **SOAP** message security provides client
authentication.

> *``Remark:`` Any protection level settings of a transport are ignored if
the **SecurityMode** is set to **None**.*

\[**TcpTransportClientCredentialType**\]

> Used when **ServiceUrl** uses **TCP** binding (i.e. **URI** starts with
"**net.tcp://**") and **SecurityMode** is **Transport** or
**TransportWithMessageCredential**.

> Available options are:

> - **None** - Specifies anonymous authentication;

> - **Windows** - Specifies client authentication using **Windows**;

> - **Certificate** - Specifies client authentication using a
    certificate.

\[**HttpTransportClientCredentialType**\]

> Used when **ServiceUrl** uses **HTTP/HTTPS** binding (i.e. **URI**
starts with "**http://**" or with "**https://**") and **SecurityMode**
is **Transport** or **TransportWithMessageCredential**.

> Available options are:

> - **None** - Specifies anonymous authentication;

> - **Basic** - Specifies **Basic** authentication. For more
information, see **RFC 2617** - **HTTP.** (**Basic** and **Digest**
Authentication);

> - **Digest** - Specifies **Digest** authentication. For more
information, see **RFC 2617 - HTTP**. (**Basic** and **Digest**
Authentication);

> - **Ntlm** - Specifies client authentication using **NTLM;**

> - **Windows** - Specifies client authentication using **Windows**;

> - **Certificate** - Specifies client authentication using a
certificate;

> - **InheritedFromHost** - The authentication is inherited from the
host.

\[**MessageClientCredentialType**\]

> Used when **SecurityMode** is **Message** or **TransportWithMessageCredential**.

> Available options are:

> - **None** - Specifies anonymous authentication;

> - **Windows** - Specifies client authentication using **Windows**;

> - **UserName** - Specifies client authentication using **UserName**;

> - **Certificate** - Specifies client authentication using a 
certificate;

> - **IssuedToken** - Specifies client authentication using an issued 
token.

\[**IncludeExceptionDetailInFaults**\]

> Specifies whether to include managed exception information in the detail
of **SOAP** faults returned to the client for debugging purposes.

> *``Remark:`` Returning managed exception information to clients can be a
security risk because exception details expose information about the
internal service implementation that could be used by unauthorized
clients.*

\[**MaxBufferPoolSize**\]

> Specifies the maximum amount of memory allocated, in bytes, for the
buffer manager that manages the buffers required by endpoints using this
binding.

\[**MaxReceivedMessageSize**\]

Specifies the maximum size, in bytes, for a message that can be
processed by the binding.

\[**Reader Quotas**\]

> Defines the constraints on the complexity of **SOAP** messages that can
be processed by endpoints configured with a binding:

> - **MaxArrayLength** - A positive integer that specifies the maximum
allowed array length of data being received by **Windows
Communication Foundation** (**WCF**) from a client. The default is
**16384;**

> - **MaxBytesPerRead** - A positive integer that specifies the maximum
allowed bytes returned per read. The default is 4096;

> - **MaxDepth** - A positive integer that specifies the maximum nested
node depth per read. The default is 32;

> - **MaxNameTableCharCount** - A positive integer that specifies the
maximum characters allowed in a table name. The default is 16384;

> - **MaxStringContentLength** - A positive integer that specifies the
    maximum characters allowed in **XML** element content. The default
    is 8192.

> *``Remark:`` All limits on installation are set to the max int
(2147483647).*

\[**ValidateClientCertificateHashes**\]

> You can specify service to validate client certificate thumbprints
(hashes). To enable the thumbprint validation do the following:

> First enable validate client certificate by setting "**true**" of
the "**ValidateClientCertificateHashes**" tag:

> __Example:__

```xml
<ValidateClientCertificates>true</ValidateClientCertificates>
```

> Second describe valid certificates thumbprints by filling the 
"**ClientCertificateHashes**" tag:

> __Example:__

```xml
<ClientCertificateHashes>
    <string>2B88177FD972EDCFF1610A035205E995B71B7B34</string>
    <string>75D0FED71881F2141B5B6CB24801DFA554439B1C</string>
</ClientCertificateHashes>
```
> Use only capital letters without any separators between bytes (e.g.
**06C02C429B09473061B20225D592D4D4844F8F6A**).

\[**ServerCertificateHash**\]

> When using **SecurityMode** **Transport** or
**TransportWithMessageCredential** and
**TcpTransportClientCredentialType** is set to **Certificate** and
**net.tcp** **URI** prefix is used then a certificate must be provided.
The certificate must be installed in the **LocaComputer\>Personal**
certificate store. Specify which certificate to use for securing
transport using property **ServerCertificateHash** where you must
provide the certificate thumbprint.

> __Example:__

> `ServerCertificateHash="41C9655CE71838DD7ECCEB051E4BA9C388CB7F98"`

\[**PermissionRole**\]

> If **PermissionRole** is not empty, then when connecting the service
will check if the client has the requested role.

> __Example:__

> `permissionRole="Administrators"`

> Example of usage is when the **SecurityMode** is set to **Message** or
**TransportWithMessageCredential** and **MessageClientCredentialType**
is set to "**UserName**".

\[**TempFolder**\]

> **TempFolder** defines a folder where temporary files will be stored.

> *``Remark:`` The "**TempFolder**" must be in **Trusted Locations** in
**Microsoft Excel's** settings in order to be able to work with it.*

## Server Certificate Binding

If **SecurityMode** is **Transport** or
**TransportWithMessageCredential** and **HTTPS** **URI** prefix is used.
The server certificate must be registered. For that purpose use the
"**netsh**" command like:

```cmd
netsh http add sslcert ipport=0.0.0.0:29500 appid={e57e408e-c377-41e9-83d8-d1cdaae4d55c} certhash=75d0fed71881f2141b5b6cb24801dfa554439b1c clientcertnegotiation=enable
```

With "**ipport=0.0.0.0:29500**" we specify on all available addresses on
port **29500**.  
With "**appid={e57e408e-c377-41e9-83d8-d1cdaae4d55c}**" we specify our application **ID**,  
which in this case is the application **ID** of our startup binary which is  
"**&lt;InstallDir&gt;\\Smart.Reporting.PhdReportingServices.exe**". 
The application **ID** is also known as the application **GUID**, which is almost unique. With "**certhash=75d0fed71881f2141b5b6cb24801dfa554439b1c**" we describe the thumbprint of the server certificate, which to be used. With
"**clientcertnegotiation=enable**" we specify that the server will ask
the client for its certificate in order to validate the thumbprint. The
parameter "**clientcertnegotiation=enable"** will allows C/S mutually
authentication based on certificates, i.e. server side could verify the
certificate validation of the client side as well as the client side
verifying the server side. If you don't want verification for client
side, just omit the parameter

*``Remark:`` The server certificate must be trusted by the machine and
must be installed in  
"**Local Computer\\Personal**" certificate store in
order to work.*

To view the **SSL** certificate bindings you can use the command:

```cmd
C:\>netsh http show sslcert
```

The output should be something like:

```cmd
SSL Certificate bindings:
-------------------------

    IP:port                 : 0.0.0.0:29500
    Certificate Hash        : 75d0fed71881f2141b5b6cb24801dfa554439b1c
    Application ID          : {e57e408e-c377-41e9-83d8-d1cdaae4d55c}
    Certificate Store Name  : (null)
    Verify Client Certificate Revocation    : Enabled
    Verify Revocation Using Cached Client Certificate Only    : Disabled
    Usage Check    : Enabled
    Revocation Freshness Time : 0
    URL Retrieval Timeout   : 0
    Ctl Identifier          : (null)
    Ctl Store Name          : (null)
    DS Mapper Usage    : Disabled
Negotiate Client Certificate    : Enabled
```
To delete SSL certificate binding you can use the command:

```cmd
C:\>netsh http delete sslcert ipport=0.0.0.0:29500
```

You can read more about **netsh** command line utility
[here](http://technet.microsoft.com/en-us/library/cc785383(v=ws.10).aspx).

*``Remark:`` If you want a non-privileged account to run the server, you
have to add **ACL** (**Access Control Lists**) to the system.*

__Example:__

```cmd
netsh http add urlacl url=https://+:29500/ user=DOMAIN\\user
```

Pay attention to the parameter "**user**". Put whatever user you want to
assign the start server right to here. If set the parameter
**user=users**, it will grant all user account (non-privileged) to start
the app and listen on the specific **IP** address and port.

# Debugging
For debugging purposes there are additional two options, in the  
"**&lt;InstallDir&gt;\\Config\\Smart.Reporting.PhdReportingServices.Configuration.xml**"
configuration file, which can help investigating unusual application
behavior.

If you set "**DebugTraceFirstChanceExceptions"** to "**true**", then an
event handler will be attached to the application domain first chance
exceptions handler and all exceptions will be traced to the log.

__Example:__

```xml
<DebugTraceFirstChanceExceptions>true</DebugTraceFirstChanceExceptions>
```

If you set "**DebugTraceUnhandledExceptions"** to "**true**", then an
event handler will be attached to the application domain unhandled
exceptions handler and all unhandled exceptions will be traced to the
log. Also in the log will be traced if the exception is terminating or
not.

__Example:__

```xml
<DebugTraceUnhandledExceptions>true</DebugTraceUnhandledExceptions>
```

*``Remark:`` Be careful when use these options, the log files can grow
dramatically if you leave them turned on. Use only for debugging
purposes.*

# Configuring PHD Connections

**Smart Reporting Service** supports upto 3 connections to different
**Uniformance PHDs**. Each connection is configured via a separate
"**PhdConnectionSettings**" section in the service configuration file  
**&lt;InstallDir&gt;\\Config\\Smart.Reporting.PhdReportingServices.Configuration.xml**.

__Exmaple:__

```xml
<PhdConnectionSettings Host="PHD-21L2-1"
                         Port="3150"
                         Username="user"
                         Password="pass"
                         ServerVersion="RAPI200"
                         WindowsUsername=""
                         WindowsPassword=""
                         MaxRows="0"
                         MinConfidence="100"
                         StringNotAvailable="#NA"
                         StringBadValue="#BAD"
                         StringError ="#ERR"
                         StringConnectionError="#CONN"
                         ConnectionTimeout="5000"
                         RetryPeriond="1000"/>

<PhdConnectionSettings Host="PHD-21L2-2"
                         Port="3150"
                         Username="user"
                         Password="pass"
                         ServerVersion="RAPI200"
                         WindowsUsername=""
                         WindowsPassword=""
                         MaxRows="0"
                         MinConfidence="100"
                         StringNotAvailable="#NA"
                         StringBadValue="#BAD"
                         StringError ="#ERR"
                         StringConnectionError="#CONN"
                         ConnectionTimeout="5000"
                         RetryPeriond="1000"/>
```


**Configuration Attributes**

\[**Host**\]

> Host name or IP address of the **Uniformance PHD** server.

\[**Port**\]

> Connection port used

\[**Username**\]

> Username required to connect to the **Uniformance PHD** server.

\[**Password**\]

> Password required to connect to the **Uniformance PHD** server.

\[**ServerVersion**\]

> Protocol version used to connect to the **Uniformance PHD** server
(**RAPI200**, **API200**, **API150**,**DEFAULT**).

\[**WindowsUsername**\]

> **Windows** username required to connect to the **Uniformance PHD**
server.

\[**WindowsPassword**\]

> **Windows** password required to connect to the **Uniformance PHD**
server.

\[**MaxRows**\]

> Maximum allowed rows to process from the template. 0 means no
restrictions.

\[**MinConfidence**\]

> Minimum confidence for the fetched value in % (0-100). If value
confidence is below minimum, it's quality is considered as bad and
`StringBadValue` will be returned instead.

\[**StringNotAvailable**\]

> What string to return when no data is available.

\[**StringBadValue**\]

> What string to return when confidence is below minimum.

\[**StringError**\]

> What string to return when there is parsing error for example.

\[**StringConnectionError**\]

> What string to return when there is problem connecting with **PHD**
server.

\[**ConnectionTimeout**\]

> The **ConnectionTimeout** property identifies the network timeout period
used when attempting to connect to the **PHD** server. If the connection
to the **PHD** server is established within the time period, the request
is abandoned. It is in milliseconds.

\[**RetryPeriod**\]

> The **RetryPeriod** property identifies the period after a connection
timeout occurs before another connection request to the PHD server will
occur. This property is related to the ConnectionTimeout property for
the handling of network timeout conditions in communication with the
**PHD** Server. It is in milliseconds.