---
title: "-Araçlar ve uzantılar - Detached.EntityFramework EF çekirdek"
author: ErikEJ
ms.author: divega
ms.date: 01/19/2017
ms.assetid: DE1C3FEA-F618-4C11-932F-7C392A1B2C94
ms.technology: entity-framework-core
uid: core/extensions/entityframeworkcore-detached
ms.openlocfilehash: 93937eee07a9e1a0b94bd92140faec7d1753332f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="efdetachedentityframework-extension"></a><span data-ttu-id="6026d-102">EFDetached.EntityFramework uzantısı</span><span class="sxs-lookup"><span data-stu-id="6026d-102">EFDetached.EntityFramework Extension</span></span>

> [!NOTE]  
> <span data-ttu-id="6026d-103">Bu uzantı Entity Framework Çekirdek projenin bir parçası olarak korunmaz.</span><span class="sxs-lookup"><span data-stu-id="6026d-103">This extension is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="6026d-104">Bir üçüncü taraf uzantı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="6026d-104">When considering a third party extension, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

<span data-ttu-id="6026d-105">Yükler ve tüm ayrılmış varlık grafikleri (kendi alt varlıkları ve listeleri olan varlık) kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6026d-105">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="6026d-106">Tarafından neden olacak [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="6026d-106">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="6026d-107">Ayrıca bazı eklentiler için simplificate denetim ve sayfa numaralandırma gibi bazı yinelenen görevleri ekleyin olur.</span><span class="sxs-lookup"><span data-stu-id="6026d-107">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

<span data-ttu-id="6026d-108">Aşağıdaki kaynak Detached.EntityFramework ile çalışmaya başlamanıza yardımcı olur</span><span class="sxs-lookup"><span data-stu-id="6026d-108">The following resource will help you get started with Detached.EntityFramework</span></span>
* [<span data-ttu-id="6026d-109">Detached.EntityFramework GitHub deposunu</span><span class="sxs-lookup"><span data-stu-id="6026d-109">Detached.EntityFramework GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)
