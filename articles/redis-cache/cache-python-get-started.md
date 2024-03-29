<properties
	pageTitle="Come usare Cache Redis di Azure con Python | Microsoft Azure"
	description="Introduzione all'uso di Cache Redis di Azure con Python"
	services="redis-cache"
	documentationCenter=""
	authors="steved0x"
	manager="douge"
	editor="v-lincan"/>

<tags
	ms.service="cache"
	ms.devlang="python"
	ms.topic="hero-article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="05/31/2016"
	ms.author="sdanie"/>

# Come usare Cache Redis di Azure con Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.JS](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Questo argomento descrive come usare Cache Redis di Azure con Python.


## Prerequisiti

Installare [redis-py](https://github.com/andymccurdy/redis-py).


## Creare una cache Redis in Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Recuperare il nome host e le chiavi di accesso

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Abilitare l'endpoint non SSL

Alcuni client Redis non supportano SSL e per impostazione predefinita la [porta non SSL è disabilitata per le nuove istanze della cache Redis di Azure](cache-configure.md#access-ports). Al momento della stesura di questo articolo, il client [redis-py](https://github.com/andymccurdy/redis-py) non supporta SSL.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Aggiungere dati alla cache e recuperarli


	>>> import redis
	>>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
	      port=6380, db=0, password='<key>', ssl=True)
	>>> r.set('foo', 'bar')
	True
	>>> r.get('foo')
	b'bar'


Sostituire `<name>` con il nome della cache e `key` con la chiave di accesso.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png

<!---HONumber=AcomDC_0601_2016-->