---
title: "EF çekirdek verileri - kaydetme"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="saving-data"></a>Verileri Kaydetme

Her bağlam örneğine sahip bir `ChangeTracker` veritabanına yazılması gereken değişiklikleri izlemek için sorumlu. Varlık sınıflarınızı örneklerine değişiklik gibi bu değişiklikler kaydedilir `ChangeTracker` ve çağırdığınızda veritabanına yazılan `SaveChanges`. Veritabanı sağlayıcısı değişiklikleri veritabanına özgü işlemlere çevirmek için sorumlu olduğu (örneğin `INSERT`, `UPDATE`, ve `DELETE` komutları bir ilişkisel veritabanı için).
