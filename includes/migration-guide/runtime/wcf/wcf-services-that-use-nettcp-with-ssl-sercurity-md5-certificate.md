### WCF services that use NETTCP with SSL sercurity and MD5 certificate authentication

|   |   |
|---|---|
|Details|The .NET Framework 4.6 adds TLS 1.1 and TLS 1.2 to the WCF SSL default protocol list. When both client and server machines have the .NET Framework 4.6 or later installed, TLS 1.2 is used for negotiation.TLS 1.2 does not support MD5 certificate authentication. As a result, if a customer uses an MD5 certificate, the WCF client will fail to connect to the WCF service.<ul><li>[ ] Quirked // Uses some mechanism to turn the feature on or off, usually using runtime targeting, AppContext or config files. Needs to be turned on automatically for some situations.</li><li>[ ] Build-time break // Causes a break if attempted to recompile</li></ul>|
|Suggestion|You can work around this issue so that a WCF client can connect to a WCF server by doing any of the following:<ul><li>Update the certificate to not use the MD5 algorithm. This is the recommended solution.</li><li>If the binding is not dynamically configured in source code, update the application&#39;s configuration file to use TLS 1.1 or an earlier version of the protocol. This allows you to continue to use a certificate with the MD5 hash algorithm.</li></ul>
<blockquote>
[!WARNING] This workaround is not recommended, since a certificate with the MD5 hash algorithm is considered insecure.</blockquote>
The following configuration file does this:<pre><code>&lt;configuration&gt;&#13;&#10;&lt;system.serviceModel&gt;&#13;&#10;&lt;bindings&gt;&#13;&#10;&lt;netTcpBinding&gt;&#13;&#10;&lt;binding&gt;&#13;&#10;&lt;security mode= &quot;None|Transport|Message|TransportWithMessageCredential&quot; &gt;&#13;&#10;&lt;transport clientCredentialType=&quot;None|Windows|Certificate&quot;&#13;&#10;protectionLevel=&quot;None|Sign|EncryptAndSign&quot;&#13;&#10;sslProtocols=&quot;Ssl3|Tls1|Tls11&quot;&gt;&#13;&#10;&lt;/transport&gt;&#13;&#10;&lt;/security&gt;&#13;&#10;&lt;/binding&gt;&#13;&#10;&lt;/netTcpBinding&gt;&#13;&#10;&lt;/bindings&gt;&#13;&#10;&lt;/system.ServiceModel&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre><ul><li>If the binding is dynamically configured in source code, update the <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols?displayProperty=nameWithType> property to use TLS 1.1 (<xref:System.Security.Authentication.SslProtocols.Tls11?displayProperty=nameWithType> or an earlier version of the protocol in the source code.</li></ul>
<blockquote>
[!WARNING] This workaround is not recommended, since a certificate with the MD5 hash algorithm is considered insecure.</blockquote>|
|Scope|Unknown|
|Version|4.6|
|Type|Runtime|
