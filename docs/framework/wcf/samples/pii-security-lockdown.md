---
title: PII 보안 잠금
ms.date: 03/30/2017
ms.assetid: c44fb338-9527-4dd0-8607-b8787d15acb4
ms.openlocfilehash: ad4f4a024b04a028b815faedded58713e001cab0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79144268"
---
# <a name="pii-security-lockdown"></a>PII 보안 잠금
이 샘플에서는 WCF(Windows 통신 재단) 서비스의 여러 보안 관련 기능을 다음과 같은 방법을 보여 줍니다.  
  
- 서비스의 구성 파일에서 중요한 정보 암호화  
  
- 중첩된 서비스 하위 디렉터리에서 설정을 재정의할 수 없도록 구성 파일에서 요소 잠금  
  
- 추적 및 메시지 로그에서 PII(개인적으로 식별할 수 있는 정보)의 로깅 제어  
  
> [!IMPORTANT]
> 컴퓨터에 이 샘플이 이미 설치되어 있을 수도 있습니다. 계속하기 전에 다음(기본) 디렉터리를 확인하세요.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 이 디렉터리가 없는 경우 [.NET Framework 4에 대한 WCF(Windows 통신 재단) 및 WF(Windows 워크플로우 재단) 샘플로](https://www.microsoft.com/download/details.aspx?id=21459) 이동하여 모든 WCF(Windows 통신 재단) 및 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 샘플을 다운로드합니다. 이 샘플은 다음 디렉터리에 있습니다.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\SecurityLockdown`  
  
## <a name="discussion"></a>토론  
 이러한 각 기능을 별개로 사용하거나 함께 사용하여 서비스 보안의 여러 측면을 제어할 수 있습니다. 이것은 WCF 서비스 보안에 대한 확실한 가이드가 아닙니다.  
  
 .NET Framework는 데이터베이스에 연결하기 위한 연결 문자열과 같은 중요한 정보를 포함할 수 있습니다. 공유된 웹 호스팅 시나리오에서는 서비스의 구성 파일에 포함된 데이터가 노출되지 않도록 구성 파일에서 이 정보를 암호화하는 것이 바람직할 수 있습니다. .NET Framework 2.0 이상에는 Windows DPAPI(데이터 보호 응용 프로그래밍 인터페이스) 또는 RSA 암호화 공급자를 사용하여 구성 파일의 일부를 암호화하는 기능이 있습니다. DPAPI 또는 RSA를 사용하는 aspnet_regiis.exe는 구성 파일의 특정 부분을 암호화할 수 있습니다.  
  
 웹 호스팅 시나리오에서는 다른 서비스의 하위 디렉터리에 서비스를 가질 수 있습니다. 구성 값을 결정하기 위한 기본 의미 체계를 사용하면 중첩된 디렉터리의 구성 파일이 부모 디렉터리의 구성 값을 재정의할 수 있습니다. 특정 상황에서는 다양한 이유로 인해 이 기능이 적합하지 않을 수 있습니다. WCF 서비스 구성은 중첩된 구성이 재정의된 구성 값을 사용하여 중첩된 서비스를 실행할 때 예외를 생성할 수 있도록 구성 값 의 잠금을 지원합니다.  
  
 이 샘플에서는 사용자 이름 및 암호와 같은 추적 및 메시지 로그의 알려진 PII(개인적으로 식별할 수 있는 정보)의 로깅을 제어하는 방법을 보여 줍니다. 기본적으로 알려진 PII의 로깅은 사용되지 않지만 특정 상황에서는 애플리케이션 디버깅을 위해 PII의 로깅이 중요할 수 있습니다. 이 샘플은 [시작하기](../../../../docs/framework/wcf/samples/getting-started-sample.md)를 기반으로 합니다. 또한 이 샘플에서는 추적 및 메시지 로깅을 사용합니다. 자세한 내용은 추적 [및 메시지 로깅](../../../../docs/framework/wcf/samples/tracing-and-message-logging.md) 샘플을 참조하십시오.  
  
## <a name="encrypting-configuration-file-elements"></a>구성 파일 요소 암호화  
 공유된 웹 호스팅 환경에서의 보안을 위해 중요한 정보가 포함된 데이터베이스 연결 문자열과 같은 특정 구성 요소를 암호화하는 것이 바람직할 수 있습니다. 구성 요소는 .NET Framework 폴더에 있는 aspnet_regiis.exe 도구를 사용하여 암호화될 수 있습니다.예를 들어 %WINDIR%\Microsoft.NET\Framework\v4.0.20728.  
  
#### <a name="to-encrypt-the-values-in-the-appsettings-section-in-webconfig-for-the-sample"></a>샘플의 Web.config에서 appSettings 섹션의 값을 암호화하려면  
  
1. 시작 >실행을 사용하여 명령 프롬프트를 엽니다.... 입력하고 `cmd` **확인을**클릭합니다.  
  
2. `cd %WINDIR%\Microsoft.NET\Framework\v4.0.20728` 명령을 실행하여 현재 .NET Framework 디렉터리로 이동합니다.  
  
3. `aspnet_regiis -pe "appSettings" -app "/servicemodelsamples" -prov "DataProtectionConfigurationProvider"` 명령을 실행하여 Web.config 폴더에서 appSettings 구성 설정을 암호화합니다.  
  
 구성 파일의 암호화 섹션에 대한 자세한 내용은 ASP.NET[구성(보안 ASP.NET 응용 프로그램 빌드: 인증, 권한 부여 및 보안 통신)에서](https://docs.microsoft.com/previous-versions/msp-n-p/ff649248(v=pandp.10))DPAPI의 방법 및 ASP.NET 구성의 RSA 사용 방법(사용 방법: RSA 사용 ASP.NET[2.0의 구성 섹션 암호화)을](https://docs.microsoft.com/previous-versions/msp-n-p/ff650304(v=pandp.10))참조하여 확인할 수 있습니다.  
  
## <a name="locking-configuration-file-elements"></a>구성 파일 요소 잠금  
 웹 호스팅 시나리오에서는 서비스의 하위 디렉터리에 서비스를 가질 수 있습니다. 이러한 상황에서는 Machine.config의 값을 검사한 다음 디렉터리 트리를 내려가면서 부모 디렉터리의 모든 Web.config 파일과 병합하고 마지막으로 서비스가 포함된 디렉터리의 Web.config 파일을 병합함으로써 하위 디렉터리의 서비스에 대한 구성 값을 계산합니다. 대부분의 구성 요소에서는 기본적으로 하위 디렉터리의 구성 파일이 부모 디렉터리의 값을 재정의하도록 허용됩니다. 그러나 특정 상황에서는 하위 디렉터리의 구성 파일이 부모 디렉터리 구성에 설정된 값을 재정의하지 못하도록 방지하는 것이 바람직할 수 있습니다.  
  
 .NET Framework에서는 잠겨진 구성 요소를 재정의하는 구성이 런타임 예외를 throw하도록 구성 파일 요소를 잠그는 방법이 제공됩니다.  
  
 구성 파일에서 노드에 대한 `lockItem` 특성을 지정하여 구성 요소를 잠글 수 있습니다. 예를 들어, 중첩된 구성 파일의 계산기 서비스가 동작을 변경할 수 없도록 구성 파일에서 CalculatorServiceBehavior 노드를 잠그려면 다음 구성을 사용할 수 있습니다.  
  
```xml  
<configuration>  
   <system.serviceModel>  
      <behaviors>
          <serviceBehaviors>
             <behavior name="CalculatorServiceBehavior" lockItem="true">
               <serviceMetadata httpGetEnabled="True"/>
               <serviceDebug includeExceptionDetailInFaults="False" />
             </behavior>
          </serviceBehaviors>
       </behaviors>
    </system.serviceModel>  
</configuration>  
```  
  
 구성 요소 잠금은 더 세부적일 수 있습니다. 요소 목록을 `lockElements`에 대한 값으로 지정하여 하위 요소 컬렉션 내에서 요소 집합을 잠글 수 있습니다. 또한 특성 목록을 `lockAttributes`에 대한 값으로 지정하여 요소 내에서 특성 집합을 잠글 수 있습니다. 노드에서 `lockAllElementsExcept` 또는 `lockAllAttributesExcept` 특성을 지정하면 지정된 목록을 제외하고 요소 또는 특성의 전체 컬렉션을 잠글 수 있습니다.  
  
## <a name="pii-logging-configuration"></a>PII 로깅 구성  
 PII 로깅은 두 개의 스위치로 제어됩니다. 하나는 컴퓨터 관리자가 PII 로깅을 허용하거나 거부할 수 있게 하는 Machine.config에 있는 컴퓨터 수준 설정이고 다른 하나는 애플리케이션 관리자가 Web.config 또는 App.config 파일의 각 소스에 대해 PII 로깅을 설정/해제할 수 있게 하는 애플리케이션 설정입니다.  
  
 컴퓨터 전체 설정은 `enableLoggingKnownPii` Machine.config의 `true` `false` `machineSettings` 요소 또는 에 설정하여 제어됩니다. 예를 들어 다음을 통해 응용 프로그램에서 PII 로깅을 켤 수 있습니다.  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <machineSettings enableLoggingKnownPii="true" />  
    </system.serviceModel>  
</configuration>  
```  
  
> [!NOTE]
> Machine.config 파일의 기본 위치는 %WINDIR%\Microsoft.NET\Framework\v2.0.50727\CONFIG입니다.  
  
 `enableLoggingKnownPii` 특성이 Machine.config에 없을 경우 PII 로깅이 허용되지 않습니다.  
  
 애플리케이션에 대해 PII 로깅을 사용하도록 설정하려면 Web.config 또는 App.config 파일에서 소스 요소의 `logKnownPii` 특성을 `true` 또는 `false`로 설정합니다. 예를 들어, 다음은 메시지 로깅 및 추적 로깅 모두에 대해 PII 로깅을 사용하도록 설정합니다.  
  
```xml  
<configuration>  
    <system.diagnostics>  
        <sources>  
            <source name="System.ServiceModel.MessageLogging" logKnownPii="true">  
                <listeners>
                ...
                </listeners>  
            </source>  
            <source name="System.ServiceModel" switchValue="Verbose, ActivityTracing">  
            <listeners>  
        ...
            </listeners>  
            </source>  
        </sources>  
    </system.diagnostics>  
</configuration>  
```  
  
 `logKnownPii` 특성이 지정되지 않은 경우 PII가 기록되지 않습니다.  
  
 `enableLoggingKnownPii`가 `true`로 설정되고 `logKnownPii`가 `true`로 설정된 경우에만 PII가 기록됩니다.  
  
> [!NOTE]
> System.Diagnostics는 구성 파일에 나열된 첫 번째 소스를 제외한 모든 소스의 모든 특성을 무시합니다. 구성 파일의 두 번째 소스에 `logKnownPii` 특성을 추가해도 아무 효과가 없습니다.  
  
> [!IMPORTANT]
> 이 샘플을 실행하려면 Machine.config를 수동으로 수정해야 합니다. 잘못된 값이나 구문으로 Machine.config를 수정할 때 모든 .NET Framework 응용 프로그램이 실행되지 않을 수 있도록 주의해야 합니다.  
  
 또한 DPAPI 및 RSA를 사용하여 구성 파일 요소를 암호화할 수 있습니다. 자세한 내용은 다음 링크를 참조하십시오.  
  
- [보안된 ASP.NET 응용 프로그램 빌드: 인증, 권한 부여 및 보안 통신](https://docs.microsoft.com/previous-versions/msp-n-p/ff649248(v=pandp.10))  
  
- [방법: RSA를 사용하여 ASP.NET 2.0에서 구성 섹션 암호화](https://docs.microsoft.com/previous-versions/msp-n-p/ff650304(v=pandp.10))  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>샘플을 설치, 빌드 및 실행하려면  
  
1. Windows 통신 기초 [샘플에 대한 일회성 설치 절차를](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)수행했어야 합니다.  
  
2. 필요한 경우 부모 노드가 추가되도록 Machine.config를 편집하여 `enableLoggingKnownPii` 특성을 `true`로 설정합니다.  
  
3. C# 또는 Visual Basic .NET 버전의 솔루션을 빌드하려면 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)의 지침을 따릅니다.  
  
4. 단일 또는 컴퓨터 간 구성에서 샘플을 실행하려면 Windows [통신 기반 샘플 실행의 지침을 따르십시오.](../../../../docs/framework/wcf/samples/running-the-samples.md)  
  
#### <a name="to-clean-up-the-sample"></a>샘플을 정리하려면  
  
1. Machine.config를 편집하여 `enableLoggingKnownPii` 특성을 `false`로 설정합니다.  
  
## <a name="see-also"></a>참고 항목

- [AppFabric 모니터링 샘플](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
