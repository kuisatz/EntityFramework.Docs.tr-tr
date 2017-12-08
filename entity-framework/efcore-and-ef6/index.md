---
title: "EF çekirdek & EF6 karşılaştırma"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="0d8f1-102">EF çekirdek & EF6 karşılaştırma</span><span class="sxs-lookup"><span data-stu-id="0d8f1-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="0d8f1-103">Entity Framework, Entity Framework Çekirdek ve Entity Framework 6 iki sürümü vardır.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-103">There are two versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="0d8f1-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="0d8f1-104">Entity Framework 6</span></span>

<span data-ttu-id="0d8f1-105">Entity Framework 6 (EF6) bir yıllardır özellikleri ve sabitlemeyi ile denenmiş ve test edilmiş veri erişim teknolojisidir.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="0d8f1-106">Bu ilk 2008'de .NET Framework 3.5 SP1 ve Visual Studio 2008 SP1'in bir parçası olarak yayımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="0d8f1-107">Sevk olarak EF4.1 yayın başlayarak [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/) -NuGet.org şu anda en popüler paketi.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently the most popular package on NuGet.org.</span></span>

<span data-ttu-id="0d8f1-108">EF6 desteklenen bir ürün olarak devam eder ve hata düzeltmeleri ve görünmesi biraz zaman için küçük geliştirmeleri görmeye devam edersiniz.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="0d8f1-109">Entity Framework Çekirdek</span><span class="sxs-lookup"><span data-stu-id="0d8f1-109">Entity Framework Core</span></span>

<span data-ttu-id="0d8f1-110">Entity Framework Çekirdek (EF temel), Entity Framework'ün hafif, genişletilebilir ve platformlar arası bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="0d8f1-111">EF çekirdek birçok geliştirmeleri ve EF6 ile karşılaştırıldığında yeni özellikleri sunar.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="0d8f1-112">Aynı anda EF çekirdek yeni bir kod temel ve EF6 olarak olarak olgun ' dir.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="0d8f1-113">EF çekirdek geliştirici deneyimi EF6 tutar ve EF çekirdek EF6 kullanmış olan terimleri bilgili eşitleyerek şekilde en üst düzey API'leri aynı de kalır.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="0d8f1-114">Aynı anda EF çekirdek tamamen yeni bir çekirdek bileşenleri kümesi üzerinde oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="0d8f1-115">Başka bir deyişle, EF çekirdek otomatik olarak EF6 tüm özellikleri devralmaz.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="0d8f1-116">Bu özelliklerden bazıları daha az yaygın olarak kullanılan gelecek sürümlerde (örneğin, yavaş yükleniyor ve bağlantı dayanıklılığı), diğer özellikleri EF çekirdek uygulanmadı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-116">Some of these features will show up in future releases (such as lazy loading and connection resiliency), other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="0d8f1-117">Yeni, genişletilebilir ve basit çekirdek bize EF6 (örneğin, alternatif anahtarlarını ve LINQ sorgularını karma istemci/veritabanı hesaplanmasında) uygulanacak değil EF çekirdek bazı özellikleri eklemek de izin.</span><span class="sxs-lookup"><span data-stu-id="0d8f1-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys and mixed client/database evaluation in LINQ queries).</span></span>