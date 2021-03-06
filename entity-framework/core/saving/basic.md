---
title: "Temel Kaydet - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a>Temel Kaydet

Ekleme, değiştirme ve bağlamını ve varlık sınıflarını kullanarak verileri kaldırma öğrenin.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) github'da.

## <a name="adding-data"></a>Veri ekleme

Kullanım *DbSet.Add* varlık sınıflarınızı yeni örneklerini ekleme yöntemi. Veritabanında veri çağırdığınızda eklenir *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

## <a name="updating-data"></a>Verileri güncelleştirme

EF bağlam tarafından izlenen var olan bir varlığa yapılan değişiklikleri otomatik olarak algılar. Bu, yük/sorgu veritabanından varlıkları ve önceden eklendi ve veritabanına kaydedilen varlıkları içerir.

Yalnızca özelliklerine atanan değerlerini değiştirin ve ardından çağıran *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Verileri silme

Kullanım *DbSet.Remove* , sınıflar örneklerini silmek için yöntem.

Varlık veritabanında zaten varsa, sırasında silinecek *SaveChanges*. Varlık (yani, izlenir eklenen) veritabanı henüz kaydedilmedi varsa bağlamdan kaldırılır ve artık ne zaman eklenen *SaveChanges* olarak adlandırılır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Tek bir SaveChanges birden çok işlemleri

Tek bir çağrı birden çok güncelleştirme/Ekle/Kaldır işlemlere birleştirebilirsiniz *SaveChanges*.

> [!NOTE]  
> Çoğu veritabanı sağlayıcısı için *SaveChanges* işlem. Bunun anlamı tüm işlemlerin başarılı başarısız veya ve işlemleri hiçbir zaman sol kısmen uygulanır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
