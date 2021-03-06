---
title: Message Security User Name
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: c63cfc87-6b20-4949-93b3-bcd4b732b0a2
ms.openlocfilehash: 62b6f24bab1c655038ad3295f5af3dee0fa198fd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79183506"
---
# <a name="message-security-user-name"></a>Message Security User Name
이 샘플에서는 클라이언트에 대해 사용자 이름 인증과 함께 WS-Security를 사용하고 서버의 X.509v3 인증서를 사용하는 서버 인증을 요구하는 애플리케이션을 구현하는 방법을 보여 줍니다. 클라이언트와 서버 간의 모든 애플리케이션 메시지는 서명 및 암호화됩니다. 기본적으로 클라이언트에 의해 제공되는 사용자 이름과 암호는 유효한 Windows 계정에 로그온하는 데 사용됩니다. 이 샘플은 [WSHttp바인딩](../../../../docs/framework/wcf/samples/wshttpbinding.md)을 기반으로 합니다. 이 샘플은 IIS(인터넷 정보 서비스)에 의해 호스트되는 클라이언트 콘솔 프로그램(Client.exe) 및 서비스 라이브러리(Service.dll)로 구성됩니다. 이 서비스는 요청-회신 통신 패턴을 정의하는 계약을 구현합니다.  
  
> [!NOTE]
> 이 샘플의 설치 절차 및 빌드 지침은 이 항목의 끝부분에 나와 있습니다.  
  
 또한 이 샘플에서는 다음을 보여 줍니다.  
  
- 추가 권한 부여를 수행할 수 있도록 하는 Windows 계정에 대한 기본 매핑  
  
- 서비스 코드에서 호출자의 ID 정보에 액세스하는 방법  
  
 서비스는 구성 파일 Web.config를 사용하여 정의되는 서비스와 통신하기 위한 단일 끝점을 노출합니다. 끝점은 주소, 바인딩 및 계약으로 구성됩니다. 바인딩은 표준 [ \<wsHttpBinding>](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)구성되며 기본적으로 메시지 보안을 사용합니다. 이 샘플에서는 클라이언트 사용자 이름 인증을 사용하도록 표준 [ \<wsHttpBinding>](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md) 설정합니다. 동작은 서비스 인증에 사용자 자격 증명을 사용하도록 지정합니다. 서버 인증서는 [ \<서비스 자격 증명>](../../../../docs/framework/configure-apps/file-schema/wcf/servicecredentials.md) `findValue` 속성과 주체 이름에 대한 동일한 값을 포함해야 합니다.  
  
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
  
 클라이언트 엔드포인트 구성은 서비스 엔드포인트의 절대 주소, 바인딩 및 계약으로 구성됩니다. 클라이언트 바인딩은 적절한 `securityMode` 및 `authenticationMode`를 사용하여 구성됩니다. 다중 컴퓨터 구성에서 실행할 경우 서비스 엔드포인트 주소를 적절하게 변경해야 합니다.  
  
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
  
 클라이언트 구현에서는 사용할 사용자 이름과 암호를 설정합니다.  

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

 샘플을 실행하면 작업 요청 및 응답이 클라이언트 콘솔 창에 표시됩니다. 클라이언트를 종료하려면 클라이언트 창에서 Enter 키를 누릅니다.  
  
```console  
MyMachine\TestAccount  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
 MessageSecurity 샘플에 포함된 Setup.bat 배치 파일을 사용하면 인증서 기반 보안이 필요한 호스트된 애플리케이션을 실행하도록 관련 인증서가 있는 서버를 구성할 수 있습니다. 배치 파일은 두 가지 모드로 실행될 수 있습니다. 단일 컴퓨터 모드에서 배치 파일을 실행하려면 명령줄에 `setup.bat`를 입력합니다. 서비스 모드에서 실행하려면 `setup.bat service`를 입력합니다. 다중 컴퓨터 구성에서 샘플을 실행할 경우 이 모드를 사용합니다. 자세한 내용은 이 항목의 끝에 있는 설치 절차를 참조하세요.  
  
 다음은 배치 파일의 여러 다른 섹션에 대한 간략한 개요입니다.  
  
- 서버 인증서 만들기  
  
     Setup.bat 배치 파일에서 다음 행은 사용할 서버 인증서를 만듭니다.  
  
    ```bat
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
     %SERVER_NAME% 변수는 서버 이름을 지정합니다. 인증서는 LocalMachine 저장소에 저장됩니다. `setup.bat service`와 같은 서비스의 인수와 함께 Setup.bat 배치 파일을 실행할 경우 %SERVER_NAME%은 컴퓨터의 정규화된 도메인 이름을 포함하고  그렇지 않은 경우 기본적으로 localhost입니다.  
  
- 클라이언트의 신뢰할 수 있는 인증서 저장소에 서버 인증서 설치  
  
     다음 줄은 클라이언트의 신뢰할 수 있는 사용자 저장소에 서버 인증서를 복사합니다. 이 단계는 Makecert.exe에서 생성한 인증서를 클라이언트 컴퓨터에서 절대적으로 신뢰하지는 않기 때문에 필요합니다. Microsoft에서 발급한 인증서와 같이 클라이언트가 신뢰할 수 있는 루트 인증서를 기반으로 하는 인증서가 이미 있는 경우 클라이언트 인증서 저장소를 서버 인증서로 채우는 이 단계를 수행할 필요가 없습니다.  
  
    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
- 인증서의 프라이빗 키에 대한 사용 권한 부여  
  
     Setup.bat 일괄 처리 파일의 다음 줄은 LocalMachine 저장소에 저장된 서버 인증서를 ASP.NET 작업자 프로세스 계정에 액세스할 수 있도록 합니다.  
  
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
    > 미국 이외의 Windows 버전을 사용하는 경우 Setup.bat 파일을 편집하고 `NT AUTHORITY\NETWORK SERVICE` 계정 이름을 지역 별등으로 바꿔야 합니다.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>샘플을 설치, 빌드 및 실행하려면  
  
1. Windows 통신 기초 [샘플에 대한 일회성 설치 절차를](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)수행했어야 합니다.  
  
2. C# 또는 Visual Basic .NET 버전의 솔루션을 빌드하려면 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)의 지침을 따릅니다.  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>단일 컴퓨터 구성에서 샘플을 실행하려면  
  
1. Makecert.exe 및 FindPrivateKey.exe가 있는 폴더가 경로에 포함되는지 확인합니다.  
  
2. 관리자 권한으로 열린 Visual Studio용 개발자 명령 프롬프트에서 샘플 설치 폴더에서 Setup.bat를 실행합니다. 이 작업은 샘플 실행에 필요한 모든 인증서를 설치합니다.  
  
    > [!NOTE]
    > Setup.bat 일괄 처리 파일은 Visual Studio에 대한 개발자 명령 프롬프트에서 실행되도록 설계되었습니다. PATH 환경 변수는 SDK가 설치되는 디렉터리를 가리켜야 합니다. 이 환경 변수는 Visual Studio에 대한 개발자 명령 프롬프트 내에서 자동으로 설정됩니다.  
  
3. 주소를 `http://localhost/servicemodelsamples/service.svc`입력하여 브라우저를 사용하여 서비스에 대한 액세스를 확인합니다.
  
4. \client\bin에서 Client.exe를 실행합니다. 클라이언트 콘솔 애플리케이션에 클라이언트 동작이 표시됩니다.  
  
5. 클라이언트와 서비스가 통신할 수 없는 경우 [WCF 샘플에 대한 문제 해결 팁을](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))참조하십시오.  
  
### <a name="to-run-the-sample-across-computers"></a>다중 컴퓨터 구성에서 샘플을 실행하려면  
  
1. 서비스 컴퓨터에 디렉터리를 만듭니다. IIS(인터넷 정보 서비스) 관리 도구를 사용하여 이 디렉터리에 servicemodelsamples라는 가상 애플리케이션을 만듭니다.  
  
2. \inetpub\wwwroot\servicemodelsamples에서 서비스 컴퓨터의 가상 디렉터리로 서비스 프로그램 파일을 복사합니다. \bin 하위 디렉터리에 파일을 복사해야 합니다. Setup.bat 및 Cleanup.bat 파일도 서비스 컴퓨터로 복사합니다.  
  
3. 클라이언트 컴퓨터에 클라이언트 이진 파일용 디렉터리를 만듭니다.  
  
4. 클라이언트 프로그램 파일을 클라이언트 컴퓨터의 클라이언트 디렉터리로 복사합니다. Setup.bat, Cleanup.bat 및 ImportServiceCert.bat 파일도 클라이언트로 복사합니다.  
  
5. 서버에서 관리자 `setup.bat service` 권한으로 열린 Visual Studio용 개발자 명령 프롬프트에서 실행합니다. `service` 인수를 실행하면 `setup.bat` 컴퓨터의 정규화된 도메인 이름으로 서비스 인증서가 생성되고 서비스 인증서를 Service.cer라는 파일로 내보냅니다.  
  
6. 컴퓨터의 정규화된 도메인 이름과 일치하는 새 인증서 이름이 serviceCertificate 요소의 findValue 특성에 반영되도록 Web.config를 편집합니다`.`  
  
7. 서비스 디렉터리에서 클라이언트 컴퓨터의 클라이언트 디렉터리로 Service.cer 파일을 복사합니다.  
  
8. 클라이언트 컴퓨터의 Client.exe.config 파일에서 엔드포인트의 주소 값을 서비스의 새 주소와 일치하도록 변경합니다.  
  
9. 클라이언트에서 Visual Studio에 대한 개발자 명령 프롬프트에서 ImportServiceCert.bat를 관리자 권한으로 열어 실행합니다. 이 작업은 Service.cer 파일의 서비스 인증서를 CurrentUser - TrustedPeople 저장소로 가져옵니다.  
  
10. 클라이언트 컴퓨터의 명령 프롬프트에서 Client.exe를 실행합니다. 클라이언트와 서비스가 통신할 수 없는 경우 [WCF 샘플에 대한 문제 해결 팁을](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))참조하십시오.  
  
### <a name="to-clean-up-after-the-sample"></a>샘플 실행 후 정리를 수행하려면  
  
- 샘플 실행을 완료한 후 샘플 폴더에서 Cleanup.bat를 실행합니다.  
  
    > [!NOTE]
    > 다중 컴퓨터 구성에서 이 샘플을 실행할 경우에는 이 스크립트로 클라이언트의 서비스 인증서를 제거할 수 없습니다. 컴퓨터에서 인증서를 사용하는 WCF(Windows 통신 재단) 샘플을 실행한 경우 CurrentUser - TrustedPeople 저장소에 설치된 서비스 인증서를 지워야 합니다. 이를 수행하려면 `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` 명령을 사용합니다(예: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`).  
