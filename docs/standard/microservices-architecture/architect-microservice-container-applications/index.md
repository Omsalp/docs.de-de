---
title: Architekturcontainer und auf Microservice basierende Anwendungen
description: ".NET-Microservices-Architektur für .NET-Containeranwendungen | Architekturcontainer und auf Microservice basierende Anwendungen"
keywords: Docker, Microservices, ASP.NET, Container
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.translationtype: HT
ms.sourcegitcommit: 9bb64ea7199f5699ff166d1affb7f8126dcc6612
ms.openlocfilehash: 360b4745e768a751154f3f1445ffb0bf5b62c825
ms.contentlocale: de-de
ms.lasthandoff: 09/05/2017

---
# <a name="architecting-container--and-microservice-based-applications"></a>Architekturcontainer und auf Microservice basierende Anwendungen

 *Microservices bieten zahlreiche Vorteile, stellen aber auch große neue Herausforderungen dar. Microservice-Architekturmuster sind eine Grundvoraussetzung für das Erstellen einer auf Microservice basierenden Anwendung.*

In diesem Leitfaden haben Sie bereits die grundlegenden Konzepte für Container und Docker gelernt. Dabei handelt es sich um die mindestens erforderlichen Informationen, die Sie für die ersten Schritte mit Containern benötigen. Aber obwohl Container hilfreich und besonders für Microservices geeignet sind, sind sie für eine Microservice-Architektur nicht erforderlich. Auch viele architektonische Konzepte in diesem Abschnitt zur Architektur können ohne Container angewendet werden. Der Schwerpunkt dieses Leitfadens liegt jedoch auf der Schnittmenge beider Methoden, da die Wichtigkeit der Container bereits eingeführt wurde.

Unternehmensanwendungen können komplex sein. Oft bestehen sie aus mehreren Diensten statt aus einer einzelnen, dienstbasierten Anwendungen. Für diese Fälle müssen Sie zusätzliche architektonische Ansätze kennen, z.B. Microservices, bestimmte DDD-Muster (Domain-Driven Design) und Orchestrierungskonzepte für Container. Beachten Sie, dass dieses Kapitel nicht nur Microservices auf Containern, sondern auch Containeranwendungen beschreibt.

## <a name="container-design-principles"></a>Richtlinien für das Containerdesign

Im Containermodell stellt eine Instanz eines Containerimages einen einzelnen Prozess dar. Indem ein Containerimage als Prozessgrenze definiert wird, können Sie Grundtypen erstellen, die zum Skalieren oder Stapeln des Prozesses verwendet werden können.

Wenn Sie ein Containerimage entworfen haben, wird Ihnen die Definition [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/) in der Dockerfile angezeigt. Dadurch wird der Prozess definiert, dessen Lebensdauer die Lebensdauer des Containers steuert. Wenn der Prozess abgeschlossen ist, endet der Lebenszyklus des Containers. Container können langlebige Prozesse wie Webserver, aber auch kurzlebige Prozesse wie Batchaufträge (früher als Azure [WebJobs](https://docs.microsoft.com/azure/app-service-web/websites-webjobs-resources) implementiert) darstellen.

Wenn der Prozess fehlschlägt, wird der Container angehalten und der Orchestrator übernimmt seinen Platz. Wenn der Orchestrator dafür konfiguriert wurde, fünf Instanzen auszuführen und eine davon fehlschlägt, erstellt der Orchestrator eine andere Containerinstanz, um den fehlgeschlagenen Prozess zu ersetzen. In einem Batchauftrag wird der Prozess mit Parametern gestartet. Wenn der Prozess abgeschlossen ist, ist die Arbeit abgeschlossen.

Es gibt Szenarios, in denen mehrere Prozesse in einem einzigen Container ausgeführt werden sollen. Da es nur einen Einstiegspunkt pro Container geben kann, können Sie für dieses Szenario ein Skript innerhalb des Containers ausführen, das so viele Programme wie benötigt startet. Sie können beispielsweise [Supervisor](http://supervisord.org/) oder ein ähnliches Tool verwenden, um mehrere Prozesse innerhalb eines einzigen Containers zu starten. Obwohl es Architekturen gibt, die mehrere Prozesse pro Container enthalten können, ist dieser Ansatz nicht üblich.


>[!div class="step-by-step"]
[Zurück] (../net-core-net-framework-containers/official-net-docker-images.md) [Weiter] (containerize-monolithic-applications.md)

