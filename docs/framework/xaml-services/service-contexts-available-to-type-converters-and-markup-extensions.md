---
title: "Service Contexts Available to Type Converters and Markup Extensions | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "XAML [XAML Services], type converter services how-to"
ms.assetid: b4dad00f-03da-4579-a4e9-d8d72d2ccbce
caps.latest.revision: 13
author: "wadepickett"
ms.author: "wpickett"
manager: "wpickett"
caps.handback.revision: 13
---
# Service Contexts Available to Type Converters and Markup Extensions
Autoren der Typen, die Typ\-Konverter und Markuperweiterungsverwendungen erfordern häufig Kontextinformationen dazu, wo sich eine Verwendung im Markup oder in der umgebenden Objektdiagrammstruktur befindet. Informationen können erforderlich sein, damit das bereitgestellte Objekt ordnungsgemäß instanziiert wird oder Objektverweise zu vorhandenen Objekten im Objektdiagramm hergestellt werden können. Wenn Sie .NET Framework\-XAML\-Dienste verwenden, wird der Kontext, der möglicherweise als eine Reihe von Dienstschnittstellen verfügbar gemacht werden muss. Der Typkonverter oder der Markuperweiterungsunterstützungscode können eine Abfrage für einen Dienstkontext für Anbieter ausführen, die verfügbar ist und von <xref:System.Xaml.XamlObjectWriter> oder verwandten Typen übergeben wird. Der XAML\-Schemakontext ist direkt über einen solchen Dienst verfügbar. In diesem Thema wird beschrieben, wie von einer Wertkonverterimplementierung auf Dienstkontexte zugegriffen werden kann, außerdem werden normalerweise verfügbare Dienste und ihre Rollen aufgeführt.  
  
<a name="obtaining_services"></a>   
## Abrufen von Diensten  
 Als Implementierer eines Wertkonverters benötigen Sie häufig Zugriff auf einen Kontexttypen, auf den der Wertkonverter angewendet wird. Dieser Kontext kann möglicherweise  Informationen wie z. B. den aktiven XAML\-Schemakontext, den Zugriff auf die Typsystemzuordnung enthalten, die der XAML\-Schemakontext und der XAML\-Objektwriter bereitstellen usw. Die für eine Markuperweiterung oder eine Typenkonverter\-Implementierung verfügbaren Dienste werden über die Kontextparameter übermittelt, die Teil der Signatur der einzelnen virtuellen Methoden sind. In jedem Fall haben Sie <xref:System.IServiceProvider> im Kontext implementiert und können <xref:System.IServiceProvider.GetService%2A?displayProperty=fullName> aufrufen, um einen Dienst anzufordern.  
  
<a name="services_for_a_markup_extension"></a>   
## Dienste für eine Markuperweiterung  
 <xref:System.Windows.Markup.MarkupExtension> verfügt nur über eine virtuelle Methode, <xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A>. Der Zweck des `serviceProvider`\-Eingabeparameters besteht darin, zu definieren, wie die Dienste an Implementierungen kommuniziert werden, wenn die Markuperweiterung von einem XAML\-Prozessor aufgerufen wird. Der folgende Pseudocode veranschaulicht, wie eine Markuperweiterungsimplementierung Dienste in <xref:System.Windows.Markup.MarkupExtension.ProvideValue%2A> abfragen kann:  
  
```  
public override object ProvideValue(IServiceProvider serviceProvider) { ... // Get the IXamlTypeResolver from the service provider if (serviceProvider == null) { throw new ArgumentNullException("serviceProvider"); } IXamlTypeResolver xamlTypeResolver = serviceProvider.GetService(typeof(IXamlTypeResolver)) as IXamlTypeResolver; if (xamlTypeResolver == null) { throw new ArgumentException("IXamlTypeResolver")); } ... }  
```  
  
<a name="services_for_a_type_converter"></a>   
## Dienste für einen Typkonverter  
 <xref:System.ComponentModel.TypeConverter> verfügt über vier virtuelle Methoden, die einen Dienstkontext verwenden und XAML\-Verwendungen unterstützen. Jede dieser Methoden übergibt einen `context`\-Eingabeparameter. Dieser Parameter ist vom Typ <xref:System.ComponentModel.ITypeDescriptorContext>, diese Schnittstelle erbt jedoch <xref:System.IServiceProvider>, und es ist daher eine <xref:System.IServiceProvider.GetService%2A> \-Methode, die für Typkonverterimplementierungen verfügbar sind.  
  
 Der folgende Pseudocode veranschaulicht, wie eine Typkonverter\-Implementierung für XAML\-Verwendungen Dienste in einer seiner Überschreibungen abfragen kann, in diesem Fall <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>:  
  
```  
public override object ConvertFrom(ITypeDescriptorContext typeDescriptorContext, CultureInfo cultureInfo, object source) { IRootObjectProvider rootProvider = typeDescriptorContext.GetService(typeof(IRootObjectProvider)) as IRootObjectProvider; if (rootProvider != null && source is String) { //return something, else ... } throw GetConvertFromException(source); }  
```  
  
<a name="services_for_a_value_serializer"></a>   
## Dienste für ein Wertserialisierungsprogramm  
 Für den Wertserialisierungsprogrammkontext verwenden Sie einen Diensttyp für Anbieter, der spezifisch für die <xref:System.Windows.Markup.ValueSerializer> \-Klasse ist, <xref:System.Windows.Markup.IValueSerializerContext>. Dieser Kontext wird an Überschreibungen der vier <xref:System.Windows.Markup.ValueSerializer> virtuellem Methoden übergeben. Rufen Sie <xref:System.IServiceProvider.GetService%2A> aus dem Kontext auf, um Dienste abzurufen.  
  
<a name="using_the_xaml_service_provider_contexts"></a>   
## Verwenden der XAML\-Dienstanbieterkontexte  
 Der Dienstanbieter für den <xref:System.IServiceProvider.GetService%2A>\-Zugriff auf XAML\-Dienste, die für Markuperweiterungen oder Typkonverter verfügbar sind, wird als interne Klasse implementiert, die nur über die Schnittstelle verfügbar gemacht wird und im entsprechenden Kontext übergeben wird. Wenn ein XAML\-Verarbeitungsvorgang in den .NET Framework\-XAML\-Dienste\-Standardimplementierungen des Lade\- oder Speicherpfads das relevante Markup oder die Konvertermethoden aufruft, für die ein Dienstkontext erforderlich ist, wird dieses interne Objekt übergeben. Abhängig von der Situation stellt der Systemdienstkontext entweder `MarkupExtensionContext` oder `TextSyntaxContext` bereit, die Einzelheiten dieser beiden Klassen sind jedoch intern. Die Interaktion mit diesen Klassen ist darauf beschränkt, daraus über <xref:System.IServiceProvider.GetService%2A> Dienste anzufordern.  
  
<a name="available_systemxaml_services"></a>   
## Verfügbare Dienste aus dem .NET Framework XAML\-Dienstkontext  
 .NET Framework\-XAML\-Dienste definieren Dienste für Markuperweiterungen, Typkonverter, Wertserialisierungsprogramme und potenziell andere Verwendungen. In den folgenden Abschnitten wird jeder dieser Dienste beschrieben, und darin ist ein Leitfaden zur Verwendung einer Implementierung enthalten.  
  
### IServiceProvider  
 **Referenzdokumentation**: <xref:System.IServiceProvider>  
  
 **Relevant für:** Grundlegende Vorgänge einer dienstbasierten Infrastruktur in .NET Framework, damit Sie <xref:System.IServiceProvider.GetService%2A?displayProperty=fullName> aufrufen können.  
  
### ITypeDescriptorContext  
 **Referenzdokumentation**: <xref:System.ComponentModel.ITypeDescriptorContext>  
  
 Wird von <xref:System.IServiceProvider> abgeleitet. Diese Klasse stellt den Kontext in den <xref:System.ComponentModel.TypeConverter>\-Standardsignaturen dar; <xref:System.ComponentModel.TypeConverter> ist eine Klasse, die seit .NET Framework 1.0 vorhanden ist. Es ist ein Vorläufer des XAML\- und XAML\- <xref:System.ComponentModel.TypeConverter>\-Szenarios für die Zeichenfolgewert\-Typkonvertierung. Im .NET Framework\-XAML\-Dienste\-Kontext werden Methoden von <xref:System.ComponentModel.TypeConverter> explizit implementiert. Das explizite Verhalten der Implementierung gibt Aufrufern an, dass die <xref:System.ComponentModel.ITypeDescriptorContext> \-API für XAML\-Typsysteme oder zum Lesen oder Schreiben von Objekten aus XAML nicht relevant ist.<xref:System.ComponentModel.ITypeDescriptorContext.Container%2A>, <xref:System.ComponentModel.ITypeDescriptorContext.Instance%2A>, und <xref:System.ComponentModel.ITypeDescriptorContext.PropertyDescriptor%2A> geben in der Regel `null` aus den .NET Framework\-XAML\-Dienste\-Kontexten zurück.  
  
### IValueSerializerContext  
 **Referenzdokumentation**: <xref:System.Windows.Markup.IValueSerializerContext>  
  
 Leitet sich von <xref:System.ComponentModel.ITypeDescriptorContext> ab und stützt sich auch auf explizite Implementierungen, um falsche Auswirkungen zum XAML\-Typsystem zu unterdrücken. Unterstützt die statischen Nachschlage\-Hilfsmethoden unter <xref:System.Windows.Markup.ValueSerializer>.  
  
### IXamlTypeResolver  
 **Referenzdokumentation**: <xref:System.Windows.Markup.IXamlTypeResolver>  
  
 **Definiert durch:**  <xref:System.Windows.Markup> System.Xaml\-Assembly\-Namespace  
  
 **Relevant für:** Ladepfad\-Szenarios sowie die Interaktion mit dem XAML\-Schemakontext  
  
 **Dienst\-API:**  <xref:System.Windows.Markup.IXamlTypeResolver.Resolve%2A>  
  
 Kann die XAML\-zu\-CLR\-Typzuordnung beeinflussen, die erforderlich ist, wenn der XAML\-Writer ein CLR\-Objekt in einem Objektdiagramm erstellt.<xref:System.Windows.Markup.IXamlTypeResolver.Resolve%2A> verarbeitet eine potenziell durch ein Präfix qualifizierte Zeichenfolge, die einem XAML\-Typnamen \(<xref:System.Xaml.XamlType.Name%2A?displayProperty=fullName>\) entspricht, und gibt eine CLR <xref:System.Type> zurück. Das Auflösen von Typen hängt in der Regel stark vom XAML\-Schemakontext ab. Nur der XAML\-Schemakontext beachtet Überlegungen wie z. B. welche Assemblys geladen werden und auf welche dieser Assemblys zur Typauflösung zugegriffen werden soll.  
  
### IUriContext  
 **Referenzdokumentation**: <xref:System.Windows.Markup.IUriContext>  
  
 **Definiert durch:**  <xref:System.Windows.Markup> System.Xaml\-Assembly\-Namespace  
  
 **Relevant für:** Verarbeitung des Ladepfads und Speicherpfads von Elementwerten, bei denen es sich um URIs oder `x:Uri`\-Werte handelt.  
  
 **Dienst\-API:**  <xref:System.Windows.Markup.IUriContext.BaseUri%2A>  
  
 Dieser Dienst meldet ggf. einen global verfügbaren URI\-Stamm. Der URI\-Stamm kann verwendet werden, um relative URIs in absolute URIs oder umgekehrt aufzulösen. Dieses Szenario ist vor allem für Anwendungsdienste relevant, die von einem bestimmten Framework oder Funktionen einer häufig verwendeten Stammelementklasse in einem Framework verfügbar gemacht werden. Der Basis\-URI kann als XAML\-Readereinstellung eingerichtet werden, die dann über den XAML\-Objekt\-Writer übergeben und von diesem Dienst gemeldet wird.  
  
### IAmbientProvider  
 **Referenzdokumentation**: <xref:System.Xaml.IAmbientProvider>  
  
 **Definiert durch:**  <xref:System.Xaml> namespace, System.Xaml assembly  
  
 **Relevant für:** Verarbeitung des Ladepfads und Typsuchen\- Verzögerungen oder Optimierungen.  
  
 **Dienst\-APIs:**  <xref:System.Xaml.IAmbientProvider.GetAllAmbientValues%2A>, 3 weitere.  
  
 Das Umgebungskonzept in XAML ist eine Technik zum Kennzeichnen eines bestimmten Elements eines Typs als Umgebung. Alternativ kann ein Typ eine Umgebung sein, damit alle Eigenschaftswerte, die eine Instanz des Typs enthält, die als Umgebungseigenschaften berücksichtigt werden soll. Markuperweiterungen oder Typkonverter, die sich weiter unten im XAML\-Knotenstream befinden und Nachfolger im Objektdiagramm sind, können zur Ladezeit auf die Umgebungseigenschaft oder Typinstanz zugreifen; oder sie können Kenntnisse der Umgebungsstruktur zur Speicherzeit verwenden. Dies kann den Grad der Qualifizierung beeinflussen, der zum Auflösen von Typen für andere Dienste erforderlich ist, wie z. B. für <xref:System.Windows.Markup.IXamlTypeResolver> oder `x:Type`. Siehe auch <xref:System.Xaml.AmbientPropertyValue>.  
  
### IXamlSchemaContextProvider  
 **Referenzdokumentation**: <xref:System.Xaml.IXamlSchemaContextProvider>  
  
 **Definiert durch:**  <xref:System.Xaml> namespace, System.Xaml assembly  
  
 **Relevant für:** Ladepfad und Vorgänge, die einen XAML\-Typ in einen Unterstützungstyp auflösen müssen.  
  
 **Dienst\-API:**  <xref:System.Xaml.IXamlSchemaContextProvider.SchemaContext%2A>  
  
 Der XAML\-Schemakontext ist für alle Vorgänge zur Ladeverzögerung erforderlich, da derselbe Schemakontext in dem verzögerten Bereich fungieren muss, um den verzögerten Inhalt zu integrieren. Weitere Informationen zur Rolle des XAML\-Schemakontexts finden Sie unter [XAML Services](../../../docs/framework/xaml-services/index.md).  
  
### IRootObjectProvider  
 **Referenzdokumentation**: <xref:System.Xaml.IRootObjectProvider>  
  
 **Definiert durch:**  <xref:System.Xaml> namespace, System.Xaml assembly  
  
 **Relevant für:** Ladepfad.  
  
 **Dienst\-API:**  <xref:System.Xaml.IRootObjectProvider.RootObject%2A>  
  
 Der Dienst ist für Anwendungsdienste relevant, die von einem bestimmten Framework oder Funktionen einer häufig verwendeten Stammelementklasse in einem Framework verfügbar gemacht werden. Ein Szenario zum Abrufen des Stammobjekts ist das Verknüpfen von CodeBehind und Ereignisverbindungen. Angenommen, die WPF\-Implementierung von `x:Class` dient für die Markupkompilierung und das Verknüpfen eines Ereignishandler\-Attributs, das an einer beliebigen anderen Position im XAML\-Markup gefunden wird. Der Verbindungspunkt von Markup und CodeBehind definiert Teilklassen für die Markupkompilierung im Stammelement.  
  
### IXamlNamespaceResolver  
 **Referenzdokumentation**: <xref:System.Xaml.IXamlNamespaceResolver>  
  
 **Definiert durch:**  <xref:System.Xaml> namespace, System.Xaml assembly  
  
 **Relevant für:** Ladepfad, Speicherpfad.  
  
 **Dienst\-API:** <xref:System.Xaml.IXamlNamespaceResolver.GetNamespace%2A> für den Ladepfad, <xref:System.Xaml.IXamlNamespaceResolver.GetNamespacePrefixes%2A> für den Speicherpfad.  
  
 <xref:System.Xaml.IXamlNamespaceResolver> ist ein Dienst, der einen XAML\-Namespacebezeichner zurückgeben kann\/URI basierend auf dem Präfix, wie in dem ursprünglichen XAML\-Markup zugeordnet.  
  
### IProvideValueTarget  
 **Referenzdokumentation**: <xref:System.Windows.Markup.IProvideValueTarget>  
  
 **Definiert durch:**  <xref:System.Windows.Markup> System.Xaml\-Assembly\-Namespace  
  
 **Relevant für:** Ladepfad und Speicherpfad.  
  
 **Dienste\-APIs:**  <xref:System.Windows.Markup.IProvideValueTarget.TargetObject%2A>, <xref:System.Windows.Markup.IProvideValueTarget.TargetProperty%2A>.  
  
 Mit <xref:System.Windows.Markup.IProvideValueTarget> kann ein Typkonverter oder die Markuperweiterung den Kontext dazu abrufen, wo sie zur Ladezeit fungieren. Implementierungen könnten diesen Kontext verwenden, um eine Verwendung ungültig zu machen. WPF verfügt innerhalb einiger der  Markuperweiterungen wie z. B. <xref:System.Windows.DynamicResourceExtension> über eine Logik. Die Logik überprüft die <xref:System.Windows.Markup.IProvideValueTarget.TargetProperty%2A>, um sicherzustellen, dass die Erweiterung nur zum Festlegen von Abhängigkeitseigenschaften \(oder eine kurze Liste mit anderen Eigenschaften ohne Abhängigkeiten\) verwendet wird.  
  
### IXamlNameResolver  
 **Referenzdokumentation**: <xref:System.Xaml.IXamlNameResolver>  
  
 **Definiert durch:**  <xref:System.Xaml> namespace, System.Xaml assembly  
  
 **Relevant für:** Definition des Ladepfadobjektdiagramms, Auflösen von durch `x:Name`, `x:Reference` oder Framework\-spezifischen Techniken identifizierten Objekten.  
  
 **Dienst\-APIs:**  <xref:System.Xaml.IXamlNameResolver.Resolve%2A>; andere APIs für erweiterte Szenarios, z. B. den Umgang mit Vorwärtsverweisen.  
  
 Die .NET Framework\-XAML\-Dienste\-Implementierung der `x:Reference`\-Behandlung beruht auf diesem Dienst. Bestimmte Frameworks oder Tools, die die Frameworknutzung dieses Diensts für die `x:Name`\-Verarbeitung oder entsprechende \(<xref:System.Windows.Markup.RuntimeNamePropertyAttribute> attributierte\)\-Eigenschaftsverarbeitung unterstützen.  
  
### IDestinationTypeProvider  
 **Referenzdokumentation**: <xref:System.Xaml.IDestinationTypeProvider>  
  
 **Definiert durch:**  <xref:System.Xaml> namespace, System.Xaml assembly  
  
 **Relevant für:** Ladepfadauflösung von indirekten CLR\-Typinformationen.  
  
 **Dienst\-API:** <xref:System.Xaml.IDestinationTypeProvider.GetDestinationType%2A>  
  
 Weitere Informationen finden Sie unter <xref:System.Xaml.IDestinationTypeProvider>.  
  
## Siehe auch  
 <xref:System.Windows.Markup.MarkupExtension>   
 <xref:System.Xaml.XamlObjectWriter>   
 [Markup Extensions for XAML Overview](../../../docs/framework/xaml-services/markup-extensions-for-xaml-overview.md)   
 [Type Converters for XAML Overview](../../../docs/framework/xaml-services/type-converters-for-xaml-overview.md)