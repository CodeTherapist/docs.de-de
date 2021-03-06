---
title: Nachrichtensicherheit – Benutzername
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: c63cfc87-6b20-4949-93b3-bcd4b732b0a2
ms.openlocfilehash: c8be13743de6110658588aa983fd5da0397c5cb0
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2018
ms.locfileid: "50183424"
---
# <a name="message-security-user-name"></a>Nachrichtensicherheit – Benutzername
Dieses Beispiel zeigt, wie eine Anwendung implementiert wird, die WS-Sicherheit mit Benutzernamenauthentifizierung für den Client verwendet und eine Serverauthentifizierung über das X.509v3-Zertifikat des Servers erfordert. Alle Anwendungsnachrichten zwischen dem Client und dem Server werden signiert und verschlüsselt. Standardmäßig werden ein vom Client angegebener Benutzername und ein Kennwort zum Anmelden bei einem gültigen Windows-Konto verwendet. Dieses Beispiel basiert auf der [WSHttpBinding](../../../../docs/framework/wcf/samples/wshttpbinding.md). Das Beispiel besteht aus einem Clientkonsolenprogramm (Client.exe) und einer von IIS (Internet Information Services, Internetinformationsdienste) gehosteten Dienstbibliothek (Service.dll). Der Dienst implementiert einen Vertrag, der ein Anforderungs-Antwort-Kommunikationsmuster definiert.  
  
> [!NOTE]
>  Die Setupprozedur und die Buildanweisungen für dieses Beispiel befinden sich am Ende dieses Themas.  
  
 Dieses Beispiel veranschaulicht darüber hinaus:  
  
-   Die Standardzuordnung zu Windows-Konten, sodass zusätzliche Autorisierung ausgeführt werden kann.  
  
-   Wie auf die Identitätsinformationen der Aufrufer vom Dienstcode aus zugegriffen wird.  
  
 Der Dienst macht einen einzigen Endpunkt zur Kommunikation mit dem Dienst verfügbar. Dieser wird mit der Konfigurationsdatei "Web.config" definiert. Der Endpunkt besteht aus einer Adresse, einer Bindung und einem Vertrag. Die Bindung konfiguriert ist, mit einer normalen [ \<WsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md), die standardmäßig nachrichtensicherheit. In diesem Beispiel wird die Standard- [ \<WsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md) auf die Verwendung der clientbenutzernamenauthentifizierung festgelegt. Das Verhalten gibt an, dass zur Dienstauthentifizierung die Benutzeranmeldeinformationen verwendet werden sollen. Das Serverzertifikat muss den gleichen Wert für den Betreffnamen wie enthalten die `findValue` -Attribut in der [ \<ServiceCredentials >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md).  
  
```xml  
<system.serviceModel>  
  <protocolMapping>  
    <add scheme="http" binding="wsHttpBinding" />  
  </protocolMapping>  
  <bindings>  
    <wsHttpBinding>  
      <!--   
      This configuration defines the security mode as Message and   
      the clientCredentialType as Username.  
      By default, Username authentication attempts to authenticate the provided  
      username as a Windows computer or domain account.  
      -->  
      <binding>  
        <security mode="Message">  
          <message clientCredentialType="UserName"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true.-->  
  <behaviors>  
    <serviceBehaviors>  
      <behavior>  
        <!--   
      The serviceCredentials behavior allows one to define a service certificate.  
      A service certificate is used by the service to authenticate itself to the client and to provide message protection.  
      This configuration references the "localhost" certificate installed during the setup instructions.  
      -->  
        <serviceCredentials>  
          <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
        </serviceCredentials>  
        <serviceMetadata httpGetEnabled="True"/>  
        <serviceDebug includeExceptionDetailInFaults="False" />  
      </behavior>  
    </serviceBehaviors>  
  </behaviors>  
</system.serviceModel>  
```  
  
 Die Clientendpunktkonfiguration besteht aus einer absoluten Adresse für den Dienstendpunkt, der Bindung und dem Vertrag. Die Clientbindung wird mit dem entsprechenden `securityMode` und `authenticationMode` konfiguriert. Bei Ausführung in einem computerübergreifenden Szenario muss die Endpunktadresse entsprechend geändert werden.  
  
```xml  
<system.serviceModel>  
  <client>  
    <endpoint address="http://localhost/servicemodelsamples/service.svc"   
              binding="wsHttpBinding"   
              bindingConfiguration="Binding1"   
              behaviorConfiguration="ClientCredentialsBehavior"  
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
  </client>  
  
  <bindings>  
    <wsHttpBinding>  
      <!--   
      This configuration defines the security mode as Message and   
      the clientCredentialType as Username.  
      -->  
      <binding name="Binding1">  
        <security mode="Message">  
          <message clientCredentialType="UserName"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true.-->  
  <behaviors>  
    <endpointBehaviors>  
      <behavior name="ClientCredentialsBehavior">  
        <!--   
      Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
      is in the user's Trusted People store, then it is trusted without performing a  
      validation of the certificate's issuer chain. This setting is used here for convenience so that the   
      sample can be run without having to have certificates issued by a certification authority (CA).  
      This setting is less secure than the default, ChainTrust. The security implications of this   
      setting should be carefully considered before using PeerOrChainTrust in production code.   
      -->  
        <clientCredentials>  
          <serviceCertificate>  
            <authentication certificateValidationMode="PeerOrChainTrust" />  
          </serviceCertificate>  
        </clientCredentials>  
      </behavior>  
    </endpointBehaviors>  
  </behaviors>  
</system.serviceModel>  
```  
  
 Die Clientimplementierung legt den zu verwendenden Benutzernamen und das Kennwort fest.  

```csharp
// Create a client.  
CalculatorClient client = new CalculatorClient();  
  
// Configure client with valid computer or domain account (username,password).  
client.ClientCredentials.UserName.UserName = username;  
client.ClientCredentials.UserName.Password = password.ToString();  
  
// Call GetCallerIdentity service operation.  
Console.WriteLine(client.GetCallerIdentity());  
...  
//Closing the client gracefully closes the connection and cleans up resources.  
client.Close();  
```

 Wenn Sie das Beispiel ausführen, werden die Anforderungen und Antworten für den Vorgang im Clientkonsolenfenster angezeigt. Drücken Sie im Clientfenster die EINGABETASTE, um den Client zu schließen.  
  
```  
MyMachine\TestAccount  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
 Mit der in den MessageSecurity-Beispielen enthaltenen Batchdatei "Setup.bat" können Sie den Server mit einem relevanten Zertifikat zum Ausführen einer gehosteten Anwendung konfigurieren, die serverzertifikatbasierte Sicherheit erfordert. Die Batchdatei kann in zwei Modi ausgeführt werden. Um die Batchdatei im Einzelcomputermodus auszuführen, geben Sie in der Befehlszeile `setup.bat` ein. Um sie im Dienstmodus auszuführen, geben Sie `setup.bat service` ein. Verwenden Sie diesen Modus, wenn Sie das Beispiel computerübergreifend ausführen. Weitere Informationen finden Sie in der Setupprozedur am Ende dieses Themas.  
  
 Im Folgenden wird eine kurze Übersicht über die verschiedenen Abschnitte der Batchdateien bereitgestellt.  
  
-   Erstellen des Serverzertifikats  
  
     Mit den folgenden Zeilen aus der Batchdatei "Setup.bat" wird das zu verwendende Serverzertifikat erstellt.  
  
    ```bat
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
     Die Variable %SERVER_NAME% gibt den Servernamen an. Das Zertifikat wird im LocalMachine-Speicher gespeichert. Wird die Batchdatei Setup.bat mit einem service-Argument ausgeführt (z. B. `setup.bat service`), enthält %SERVER_NAME% den vollqualifizierten Domänennamen des Computers.  Andernfalls wird standardmäßig localhost verwendet.  
  
-   Installieren des Serverzertifikats im Speicher für vertrauenswürdige Zertifikate des Clients  
  
     Die folgenden Zeilen kopieren das Serverzertifikat in den Clientspeicher für vertrauenswürdige Personen. Dieser Schritt ist erforderlich, da von "Makecert.exe" generierte Zertifikate nicht implizit vom Clientsystem als vertrauenswürdig eingestuft werden. Wenn Sie bereits über ein Zertifikat verfügen, das von einem vertrauenswürdigen Clientstammzertifikat stammt (z. B. ein von Microsoft ausgegebenes Zertifikat), ist dieser Schritt zum Füllen des Clientzertifikatspeichers mit dem Serverzertifikat nicht erforderlich.  
  
    ```  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
-   Gewähren von Berechtigungen auf dem privaten Schlüssel des Zertifikats  
  
     Die folgenden Zeilen in der Batchdatei Setup.bat sorgen dafür, dass das Serverzertifikat, das im Speicher LocalMachine gespeichert ist, für das [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)]-Arbeitsprozesskonto verfügbar ist.  
  
    ```bat
    echo ************  
    echo setting privileges on server certificates  
    echo ************  
    for /F "delims=" %%i in ('"%ProgramFiles%\ServiceModelSampleTools\FindPrivateKey.exe" My LocalMachine -n CN^=%SERVER_NAME% -a') do set PRIVATE_KEY_FILE=%%i  
    set WP_ACCOUNT=NT AUTHORITY\NETWORK SERVICE  
    (ver | findstr /C:"5.1") && set WP_ACCOUNT=%COMPUTERNAME%\ASPNET  
    echo Y|cacls.exe "%PRIVATE_KEY_FILE%" /E /G "%WP_ACCOUNT%":R  
    iisreset  
    ```  
  
    > [!NOTE]
    >  Bei Verwendung einer anderen als der englischsprachigen US-Version von Windows müssen Sie in der Datei "Setup.bat" den Kontonamen `NT AUTHORITY\NETWORK SERVICE` durch den äquivalenten Kontonamen in der entsprechenden Sprache ersetzen.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>So können Sie das Beispiel einrichten, erstellen und ausführen  
  
1.  Stellen Sie sicher, dass Sie ausgeführt haben die [Schritte der Einrichtung einmaligen Setupverfahren für Windows Communication Foundation-Beispiele](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Um die C#- oder Visual Basic .NET-Edition der Projektmappe zu erstellen, befolgen Sie die unter [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)aufgeführten Anweisungen.  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>So führen Sie das Beispiel auf demselben Computer aus  
  
1.  Vergewissern Sie sich, dass der Pfad den Ordner enthält, in dem sich die Dateien Makecert.exe und FindPrivateKey.exe befinden.  
  
2.  Führen Sie Setup.bat aus dem Beispielinstallationsordner an einer Visual Studio-Eingabeaufforderung mit Administratorrechten aus. Hiermit werden alle Zertifikate installiert, die zum Ausführen des Beispiels erforderlich sind.  
  
    > [!NOTE]
    >  Die Batchdatei Setup.bat ist darauf ausgelegt, an einer Visual Studio-Eingabeaufforderung ausgeführt zu werden. Die PATH-Umgebungsvariable muss auf das Verzeichnis zeigen, in dem das SDK installiert ist. Diese Umgebungsvariable ist innerhalb einer Visual Studio-Eingabeaufforderung automatisch festgelegt.  
  
3.  Überprüfen des Zugriffs auf den Dienst über einen Browser unter Eingabe der Adresse `http://localhost/servicemodelsamples/service.svc`.
  
4.  Starten Sie Client.exe aus dem Ordner \client\bin. In der Clientkonsolenanwendung wird Clientaktivität angezeigt.  
  
5.  Wenn der Client und der Dienst nicht kommunizieren können, finden Sie weitere Informationen unter [Troubleshooting Tips](https://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b).  
  
### <a name="to-run-the-sample-across-computers"></a>So führen Sie das Beispiel computerübergreifend aus  
  
1.  Erstellen Sie auf dem Dienstcomputer ein Verzeichnis. Erstellen Sie mithilfe des Verwaltungstools für Internetinformationsdienste für dieses Verzeichnis eine virtuelle Anwendung mit dem Namen servicemodelsamples.  
  
2.  Kopieren Sie die Dienstprogrammdateien aus \inetpub\wwwroot\servicemodelsamples in das virtuelle Verzeichnis auf dem Dienstcomputer. Stellen Sie sicher, dass Sie die Dateien in das Unterverzeichnis \bin kopieren. Kopieren Sie außerdem die Dateien Setup.bat und Cleanup.bat auf den Dienstcomputer.  
  
3.  Erstellen Sie auf dem Clientcomputer ein Verzeichnis für die Clientbinärdateien.  
  
4.  Kopieren Sie die Clientprogrammdateien in das Clientverzeichnis auf dem Clientcomputer. Kopieren Sie die Dateien Setup.bat, Cleanup.bat und ImportServiceCert.bat ebenfalls auf den Client.  
  
5.  Führen Sie auf dem Server `setup.bat service` an einer Visual Studio-Eingabeaufforderung mit Administratorrechten aus. Ausführung `setup.bat` mit der `service` Argument wird ein Dienstzertifikat mit dem vollqualifizierten Domänennamen des Computers erstellt und das Dienstzertifikat in die Datei Service.cer exportiert.  
  
6.  Bearbeiten Sie die Datei Web.config so, dass sie den neuen Zertifikatnamen (im findValue-Attribut im serviceCertificate-Element) enthält, der dem vollqualifizierten Domänennamen des Computers entspricht`.`  
  
7.  Kopieren Sie die Datei Service.cer aus dem Dienstverzeichnis in das Clientverzeichnis auf dem Clientcomputer.  
  
8.  Ändern Sie in der Datei Client.exe.config auf dem Clientcomputer den Wert für die Adresse des Endpunkts, sodass er mit der neuen Adresse Ihres Diensts übereinstimmt.  
  
9. Führen Sie auf dem Client ImportServiceCert.bat an einer Visual Studio-Eingabeaufforderung mit Administratorrechten aus. Dadurch wird das Dienstzertifikat aus der Datei Service.cer in den Speicher CurrentUser – TrustedPeople importiert.  
  
10. Starten Sie auf dem Clientcomputer Client.exe an einer Eingabeaufforderung. Wenn der Client und der Dienst nicht kommunizieren können, finden Sie weitere Informationen unter [Troubleshooting Tips](https://msdn.microsoft.com/library/8787c877-5e96-42da-8214-fa737a38f10b).  
  
### <a name="to-clean-up-after-the-sample"></a>So stellen Sie den Zustand vor Ausführung des Beispiels wieder her  
  
-   Führen Sie Cleanup.bat im Beispielordner aus, nachdem Sie die Ausführung des Beispiels abgeschlossen haben.  
  
    > [!NOTE]
    >  Wenn dieses Beispiel computerübergreifend ausgeführt wird, entfernt dieses Skript keine Dienstzertifikate auf einem Client. Wenn Sie Windows Communication Foundation (WCF)-Beispiele, die Zertifikate computerübergreifend verwenden ausgeführt haben, achten Sie darauf, dass Sie die Dienstzertifikate entfernen, die in den Speicher CurrentUser – trustedpeople installiert wurden. Verwenden Sie dazu den folgenden Befehl: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` Beispiel: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`.  
  
## <a name="see-also"></a>Siehe auch
