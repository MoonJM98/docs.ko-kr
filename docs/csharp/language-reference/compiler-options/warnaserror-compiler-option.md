---
title: -warnaserror(C# 컴파일러 옵션)
ms.date: 07/20/2015
f1_keywords:
- /warnaserror
helpviewer_keywords:
- /warnaserror compiler option [C#]
- -warnaserror compiler option [C#]
- warnaserror compiler option [C#]
ms.assetid: 04680ec3-08d6-4e2e-a274-38310e10e33c
ms.openlocfilehash: 7d43941629e933ac5a9e9c9d6a1388b6194f8d99
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503484"
---
# <a name="-warnaserror-c-compiler-options"></a>-warnaserror(C# 컴파일러 옵션)
**-warnaserror+** 옵션은 모든 경고를 오류로 처리합니다.  
  
## <a name="syntax"></a>구문  
  
```console  
-warnaserror[+ | -][:warning-list]  
```  
  
## <a name="remarks"></a>설명  
 일반적으로 경고로 보고되는 메시지가 대신 오류로 보고되며, 빌드 프로세스가 중지됩니다(출력 파일이 작성되지 않음).  
  
 기본적으로 **-warnaserror-** 가 적용되며, 경고가 발생해도 출력 파일이 생성됩니다. **-warnaserror+** 와 동일한 **-warnaserror**는 경고가 오류로 처리되도록 합니다.  
  
 필요에 따라 몇 개의 특정 경고만 오류로 처리하려는 경우 오류로 처리할 경고 번호의 쉼표로 구분된 목록을 지정할 수 있습니다. 모든 Null 허용 여부 경고 집합은 **nullable** 축약형을 사용하여 지정할 수 있습니다.
  
 컴파일러에서 표시할 경고 수준을 지정하려면 [-warn](./warn-compiler-option.md)을 사용합니다. 특정 경고를 사용하지 않으려면 [-nowarn](./nowarn-compiler-option.md)을 사용합니다.  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 개발 환경에서 이 컴파일러 옵션을 설정하려면  
  
1. 프로젝트 **속성** 페이지를 엽니다.  
  
2. **빌드** 속성 페이지를 클릭합니다.  
  
3. **경고를 오류로 처리** 속성을 수정합니다.  
  
 프로그래밍 방식으로 이 컴파일러 옵션을 설정하려면 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.TreatWarningsAsErrors>를 참조하세요.  
  
## <a name="example"></a>예제  
 `in.cs`를 컴파일하고 컴파일러에서 경고를 표시하지 않도록 합니다.  
  
```console  
csc -warnaserror in.cs  
csc -warnaserror:642,649,652,nullable in.cs  
```  
  
## <a name="see-also"></a>참고 항목

- [C# 컴파일러 옵션](./index.md)
- [프로젝트 및 솔루션 속성 관리](/visualstudio/ide/managing-project-and-solution-properties)
