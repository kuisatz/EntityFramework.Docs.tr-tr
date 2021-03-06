---
title: "Aynı DbContext türü - EF çekirdek ile birden çok modelleri arasında geçiş yapma"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Aynı DbContext türü ile birden çok modelleri arasında geçiş yapma

Yerleşik modeli `OnModelCreating` bir özellik bağlamda modelin nasıl yapılandırıldığını değiştirmek için kullanabilirsiniz. Örneğin belirli bir özelliği dışarıda tutması için kullanılabilir:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Yukarıdaki ek değişiklik yapmadan yapılması çalıştıysanız herhangi bir değer için her bir yeni içerik oluşturulduğunda ancak aynı modelin elde edebileceğiniz `IgnoreIntProperty`. Bu mekanizma EF kullanan yalnızca çağırarak performansını artırmak için önbelleğe alma modeli kaynaklanır `OnModelCreating` bir kez ve model önbelleğe alma.

Varsayılan olarak, belirtilen her bağlam için modelin türü aynı olacağını EF varsayar. Bu varsayılan uygulaması gerçekleştirmek için `IModelCacheKeyFactory` yalnızca bağlam türü içeren bir anahtar döndürür. Bu gereksinim değiştirmek için değiştirmek için `IModelCacheKeyFactory` hizmet. Yeni uygulama karşılaştırılabilir bir nesne kullanarak diğer model anahtarlar döndürmesi gerekir `Equals` modelini etkileyen tüm değişkenleri dikkate alır yöntemi:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
