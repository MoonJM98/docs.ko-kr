---
title: Windows Identity Foundation 구성 스키마
ms.date: 03/30/2017
ms.assetid: 4d4f6d76-49a5-4bad-b345-097b2e2844e9
author: BrucePerlerMS
ms.openlocfilehash: 14d596ae77019932d169e1a84732fb8522bfc46c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152725"
---
# <a name="windows-identity-foundation-configuration-schema"></a>Windows Identity Foundation 구성 스키마

이 섹션의 항목에서는 WIF(Windows Identity Foundation) 구성 스키마에 대한 정보를 제공합니다. 프레임 워크에서 노출 하는 클래스를 통해 WIF를 사용 하도록 응용 프로그램을 구성할 수도 있습니다. 이러한 클래스는 스키마의 관련 요소를 다루는 섹션에 설명되어 있습니다. 다음은 WIF 구성 스키마에 의해 노출된 기본 XML 태그 구조를 보여 줍니다. 특성은 생략됩니다. 강조 표시된 주석은 스키마의 주요 구성 요소를 나타냅니다.  
  
```xml  
<configuration>  
    <system.identityModel>  
        <!-- Service Configuration -->  
        <identityConfiguration>  
            <caches>  
                <sessionSecurityTokenCache />  
                <tokenReplayCache />  
            </caches>  

            <certificateValidation>  
                <certificateValidator />
            </certificateValidation>  

            <claimsAuthenticationManager />  

            <claimsAuthorizationManager>  
                <optionalConfigurationElement>  
            </claimsAuthorizationManager>  

            <claimTypeRequired>  
                <claimType />
            </claimTypeRequired>  

            <tokenReplayDetection />  

            <!-- Security Token Handler Collection Configuration -->  
            <securityTokenHandlers>  
                <add>  
                    <!-- Can take an optional configuration element which can be one of  
                         the following or a custom element -->  
                    <samlSecurityTokenHandlerRequirement>  
                        <nameClaimType>  
                        <roleClaimType>
                    </samlSecurityTokenHandlerRequirement>  

                    <sessionSecurityTokenHandlerRequirement />  
                    <x509SecurityTokenHandlerRequirement />  
                    <userNameSecurityTokenHandlerRequirement />  
                </add>  
                <clear />  
                <remove />  
                <securityTokenHandlerConfiguration>  
                    <audienceUris>  
                        <add>  
                        <clear>  
                        <remove>  
                    </audienceUris>  

                    <caches>  
                        <sessionSecurityTokenCache />  
                        <tokenReplayCache />  
                    </caches>  

                    <certificateValidation>  
                        <certificateValidator>
                    </certificateValidation>  

                    <issuerNameRegistry>  
                        <!-- Can take an optional configuration element which can be   
                             the <trustedIssuers> element to configure a configuration-based  
                             issuer name registry or can be a custom element -->  
                        <trustedIssuers>  
                            <add>  
                            <clear>  
                            <remove>  
                        </trustedIssuers>  
                    </issuerNameRegistry>  

                    <issuerTokenResolver />  
                    <serviceTokenResolver />  
                    <tokenReplayDetection />  
                </securityTokenHandlerConfiguration>  
            </securityTokenHandlers>  
        </identityConfiguration>  
    </system.identityModel>  

    <system.identityModel.services>  
        <!-- Federation Authentication Configuration -->  
        <federatedAuthentication>  
            <cookieHandler>  
                <chunkedCookieHandler />  
                <customCookieHandler />  
            </cookieHandler>  

            <serviceCertificate>  
                <certificateReference>  
            </serviceCertificate>  

            <wsFederation />  
        </federatedAuthentication>  
    </system.identityModel.services>  
</configuration>  
```  
  
## <a name="in-this-section"></a>섹션 내용  

[\<system.identityModel>](system-identitymodel.md)응용 프로그램에서 WIF 옵션을 사용 하기 위한 구성을 제공 합니다.  
  
[\<system.identityModel.services>](system-identitymodel-services.md)WIF를 사용 하 여 수동 페더레이션을 위한 구성을 제공 합니다. SAM(Session Authentication Module) 및 WSFAM(Federated Authentication Module)을 구성합니다.
