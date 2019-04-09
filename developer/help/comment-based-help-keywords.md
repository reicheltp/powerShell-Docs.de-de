---
title: Kommentarbasierte Hilfe-Schlüsselwörter | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: ab90ec96-77f5-42e3-9c7e-2f4156ec207f
caps.latest.revision: 6
ms.openlocfilehash: af8a151070d26ffe236800076115c964f625e572
ms.sourcegitcommit: caac7d098a448232304c9d6728e7340ec7517a71
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2019
ms.locfileid: "58058078"
---
# <a name="comment-based-help-keywords"></a><span data-ttu-id="1e763-102">Schlüsselwörter in der kommentarbasierten Hilfe</span><span class="sxs-lookup"><span data-stu-id="1e763-102">Comment-Based Help Keywords</span></span>

<span data-ttu-id="1e763-103">Dieses Thema enthält und beschreibt die Schlüsselwörter in kommentarbasierte Hilfe.</span><span class="sxs-lookup"><span data-stu-id="1e763-103">This topic lists and describes the keywords in comment-based help.</span></span>

## <a name="keywords-in-comment-based-help"></a><span data-ttu-id="1e763-104">Schlüsselwörter in befehlsbasierte Hilfe</span><span class="sxs-lookup"><span data-stu-id="1e763-104">Keywords in Comment-Based Help</span></span>

<span data-ttu-id="1e763-105">Im folgenden werden die gültigen kommentarbasierte Hilfe-Schlüsselwörter.</span><span class="sxs-lookup"><span data-stu-id="1e763-105">The following are valid comment-based Help keywords.</span></span> <span data-ttu-id="1e763-106">Sie werden in der Reihenfolge aufgeführt, in denen sie in der Regel in einem Hilfethema zusammen mit ihrem Verwendungszweck aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="1e763-106">They are listed in the order in which they typically appear in a Help topic along with their intended use.</span></span> <span data-ttu-id="1e763-107">Diese Schlüsselwörter können in beliebiger Reihenfolge in der kommentarbasierten Hilfe angezeigt werden, und sie sind nicht Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="1e763-107">These keywords can appear in any order in the comment-based Help, and they are not case-sensitive.</span></span>

<span data-ttu-id="1e763-108">Beachten Sie, dass die `.ExternalHelp` Schlüsselwort hat Vorrang vor allen anderen kommentarbasierte Hilfe-Schlüsselwörter.</span><span class="sxs-lookup"><span data-stu-id="1e763-108">Note that the `.ExternalHelp` keyword takes precedence over all other comment-based help keywords.</span></span> <span data-ttu-id="1e763-109">Wenn `.ExternalHelp` vorhanden ist, die [Microsoft.PowerShell.Commands.Get-Help](/dotnet/api/Microsoft.PowerShell.Commands.Get-Help) Cmdlet zeigt keine kommentarbasierte Hilfe, selbst wenn er eine Hilfedatei, die den Wert des Schlüsselworts entspricht nicht finden kann.</span><span class="sxs-lookup"><span data-stu-id="1e763-109">When `.ExternalHelp` is present, the [Microsoft.PowerShell.Commands.Get-Help](/dotnet/api/Microsoft.PowerShell.Commands.Get-Help) cmdlet does not display comment-based help, even when it cannot find a help file that matches the value of the keyword.</span></span>

<span data-ttu-id="1e763-110">`.Synopsis` Eine kurze Beschreibung der Funktion oder des Skripts.</span><span class="sxs-lookup"><span data-stu-id="1e763-110">`.Synopsis` A brief description of the function or script.</span></span> <span data-ttu-id="1e763-111">Dieses Schlüsselwort kann nur einmal in jedem Thema verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1e763-111">This keyword can be used only once in each topic.</span></span>

<span data-ttu-id="1e763-112">`.Description` Eine ausführliche Beschreibung der Funktion oder des Skripts.</span><span class="sxs-lookup"><span data-stu-id="1e763-112">`.Description` A detailed description of the function or script.</span></span> <span data-ttu-id="1e763-113">Dieses Schlüsselwort kann nur einmal in jedem Thema verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1e763-113">This keyword can be used only once in each topic.</span></span>

<span data-ttu-id="1e763-114">`.Parameter` *\<Parameter-Name >* die Beschreibung eines Parameters.</span><span class="sxs-lookup"><span data-stu-id="1e763-114">`.Parameter` *\<Parameter-Name>* The description of a parameter.</span></span> <span data-ttu-id="1e763-115">Sie können einschließen, eine `.Parameter` -Schlüsselwort für jeden Parameter in der Funktion oder einem Skript.</span><span class="sxs-lookup"><span data-stu-id="1e763-115">You can include a `.Parameter` keyword for each parameter in the function or script.</span></span>

<span data-ttu-id="1e763-116">Die `.Parameter` Schlüsselwörter können angezeigt werden, in beliebiger Reihenfolge in den Kommentarblock, aber die Reihenfolge der Parameter in der `Param` -Anweisung oder Funktionsdeklaration, bestimmt die Reihenfolge, die in der die Parameter im Hilfethema angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1e763-116">The `.Parameter` keywords can appear in any order in the comment block, but the order in which the parameters appear in the `Param` statement or function declaration determines the order in which the parameters appear in Help topic.</span></span> <span data-ttu-id="1e763-117">Um die Reihenfolge der Parameter im Hilfethema zu ändern, ändern Sie die Reihenfolge der Parameter in der `Param` -Anweisung oder Funktionsdeklaration.</span><span class="sxs-lookup"><span data-stu-id="1e763-117">To change the order of parameters in the Help topic, change the order of the parameters in the `Param` statement or function declaration.</span></span>

<span data-ttu-id="1e763-118">Sie können auch eine Beschreibung des Parameters angeben, indem ein Kommentar in der `Param` Anweisung unmittelbar vor der Parameter-Variablenname.</span><span class="sxs-lookup"><span data-stu-id="1e763-118">You can also specify a parameter description by placing a comment in the `Param` statement immediately before the parameter variable name.</span></span> <span data-ttu-id="1e763-119">Wenn Sie beide verwenden eine `Param` Anweisung Kommentar und eine `.Parameter` -Schlüsselwort, das die zugeordnete Beschreibung der `.Parameter` Schlüsselwort verwendet wird, und die `Param` Anweisung Kommentar wird ignoriert.</span><span class="sxs-lookup"><span data-stu-id="1e763-119">If you use both a `Param` statement comment and a `.Parameter` keyword, the description associated with the `.Parameter` keyword is used, and the `Param` statement comment is ignored.</span></span>

<span data-ttu-id="1e763-120">`.Example` Ein Beispielbefehl, der die Funktion bzw. das Skript, optional gefolgt von der Ausgabe des Beispiels und eine Beschreibung verwendet.</span><span class="sxs-lookup"><span data-stu-id="1e763-120">`.Example` A sample command that uses the function or script, optionally followed by sample output and a description.</span></span> <span data-ttu-id="1e763-121">Wiederholen Sie dieses Schlüsselwort für jedes Beispiel.</span><span class="sxs-lookup"><span data-stu-id="1e763-121">Repeat this keyword for each example.</span></span>

<span data-ttu-id="1e763-122">`.Inputs` Die Microsoft .NET Framework-Typen von Objekten, die an die Funktion oder einem Skript übergeben werden können.</span><span class="sxs-lookup"><span data-stu-id="1e763-122">`.Inputs` The Microsoft .NET Framework types of objects that can be piped to the function or script.</span></span> <span data-ttu-id="1e763-123">Sie können auch eine Beschreibung der Eingabeobjekte einschließen.</span><span class="sxs-lookup"><span data-stu-id="1e763-123">You can also include a description of the input objects.</span></span>

<span data-ttu-id="1e763-124">`.Outputs` Der .NET Framework-Typ, der die Objekte, die mit dem Cmdlet zurückgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="1e763-124">`.Outputs` The .NET Framework type of the objects that the cmdlet returns.</span></span> <span data-ttu-id="1e763-125">Sie können auch eine Beschreibung der zurückgegebenen Objekte einschließen.</span><span class="sxs-lookup"><span data-stu-id="1e763-125">You can also include a description of the returned objects.</span></span>

<span data-ttu-id="1e763-126">`.Notes` Weitere Informationen zu der Funktion oder ein Skript.</span><span class="sxs-lookup"><span data-stu-id="1e763-126">`.Notes` Additional information about the function or script.</span></span>

<span data-ttu-id="1e763-127">`.Link` Der Name der verwandten Thema.</span><span class="sxs-lookup"><span data-stu-id="1e763-127">`.Link` The name of a related topic.</span></span> <span data-ttu-id="1e763-128">Wiederholen Sie dieses Schlüsselwort zu jedem verwandten Thema.</span><span class="sxs-lookup"><span data-stu-id="1e763-128">Repeat this keyword for each related topic.</span></span> <span data-ttu-id="1e763-129">Dieser Inhalt wird im Abschnitt "Verwandte Links" des Hilfethemas angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1e763-129">This content appears in the Related Links section of the Help topic.</span></span>

<span data-ttu-id="1e763-130">Die `.Link` Schlüsselwort Inhalt kann auch ein Uniform Resource Identifier (URI) auf eine Onlineversion des Hilfethemas gleichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="1e763-130">The `.Link` keyword content can also include a Uniform Resource Identifier (URI) to an online version of the same Help topic.</span></span> <span data-ttu-id="1e763-131">Öffnet die Onlineversion, bei der Verwendung der `Online` Parameter von Get-Help.</span><span class="sxs-lookup"><span data-stu-id="1e763-131">The online version opens when you use the `Online` parameter of Get-Help.</span></span> <span data-ttu-id="1e763-132">Der URI muss mit "http" oder "Https" beginnen.</span><span class="sxs-lookup"><span data-stu-id="1e763-132">The URI must begin with "http" or "https".</span></span>

<span data-ttu-id="1e763-133">`.Component` Die Technologie oder Funktion, die die Funktion bzw. das Skript verwendet, oder mit dem es verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="1e763-133">`.Component` The technology or feature that the function or script uses, or to which it is related.</span></span> <span data-ttu-id="1e763-134">Dieser Inhalt wird angezeigt, wenn es sich bei den Get-Help-Befehl enthält den `Component` Parameter von Get-Help.</span><span class="sxs-lookup"><span data-stu-id="1e763-134">This content appears when the Get-Help command includes the `Component` parameter of Get-Help.</span></span>

<span data-ttu-id="1e763-135">`.Role` Die Benutzerrolle für das Hilfethema.</span><span class="sxs-lookup"><span data-stu-id="1e763-135">`.Role` The user role for the Help topic.</span></span> <span data-ttu-id="1e763-136">Dieser Inhalt wird angezeigt, wenn es sich bei den Get-Help-Befehl enthält den `Role` Parameter von Get-Help.</span><span class="sxs-lookup"><span data-stu-id="1e763-136">This content appears when the Get-Help command includes the `Role` parameter of Get-Help.</span></span>

<span data-ttu-id="1e763-137">`.Functionality` Die beabsichtigte Verwendung der Funktion.</span><span class="sxs-lookup"><span data-stu-id="1e763-137">`.Functionality` The intended use of the function.</span></span> <span data-ttu-id="1e763-138">Dieser Inhalt wird angezeigt, wenn es sich bei den Get-Help-Befehl enthält den `Functionality` Parameter von Get-Help.</span><span class="sxs-lookup"><span data-stu-id="1e763-138">This content appears when the Get-Help command includes the `Functionality` parameter of Get-Help.</span></span>

<span data-ttu-id="1e763-139">`.ForwardHelpTargetName` `<Command-Name>` Leitet das Hilfethema für den angegebenen Befehl.</span><span class="sxs-lookup"><span data-stu-id="1e763-139">`.ForwardHelpTargetName` `<Command-Name>` Redirects to the Help topic for the specified command.</span></span> <span data-ttu-id="1e763-140">Sie können die Benutzer zu allen Hilfethemen, einschließlich der Hilfethemen für eine Funktion, Skripts, Cmdlet oder Anbieter umleiten.</span><span class="sxs-lookup"><span data-stu-id="1e763-140">You can redirect users to any Help topic, including Help topics for a function, script, cmdlet, or provider.</span></span>

<span data-ttu-id="1e763-141">`.ForwardHelpCategory` `<Category>` Gibt den Hilfe-Kategorie des Elements im ForwardHelpTargetName an.</span><span class="sxs-lookup"><span data-stu-id="1e763-141">`.ForwardHelpCategory` `<Category>` Specifies the Help category of the item in ForwardHelpTargetName.</span></span> <span data-ttu-id="1e763-142">Gültige Werte sind Alias, Cmdlet, HelpFile, -Funktion, Anbieter, allgemeine, häufig gestellte Fragen, Glossar, Skriptbefehl, ExternalScript, filtern oder alle.</span><span class="sxs-lookup"><span data-stu-id="1e763-142">Valid values are Alias, Cmdlet, HelpFile, Function, Provider, General, FAQ, Glossary, ScriptCommand, ExternalScript, Filter, or All.</span></span> <span data-ttu-id="1e763-143">Verwenden Sie dieses Schlüsselwort, um Konflikte zu vermeiden, wenn Befehle mit dem gleichen Namen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="1e763-143">Use this keyword to avoid conflicts when there are commands with the same name.</span></span>

<span data-ttu-id="1e763-144">`.RemoteHelpRunspace` `<PSSession-variable>` Gibt eine Sitzung, die das Hilfethema enthält.</span><span class="sxs-lookup"><span data-stu-id="1e763-144">`.RemoteHelpRunspace` `<PSSession-variable>` Specifies a session that contains the Help topic.</span></span> <span data-ttu-id="1e763-145">Geben Sie eine Variable, die eine PSSession enthält.</span><span class="sxs-lookup"><span data-stu-id="1e763-145">Enter a variable that contains a PSSession.</span></span> <span data-ttu-id="1e763-146">Dieses Schlüsselwort wird verwendet, durch die `Export-PSSession` Cmdlet, um die Hilfethemen für die exportierten Befehle zu finden.</span><span class="sxs-lookup"><span data-stu-id="1e763-146">This keyword is used by the `Export-PSSession` cmdlet to find the Help topics for the exported commands.</span></span>

<span data-ttu-id="1e763-147">`.ExternalHelp` `<XML Help File>` Gibt den Pfad und/oder Namen einer XML-basierte Hilfedatei für das Skript oder eine Funktion.</span><span class="sxs-lookup"><span data-stu-id="1e763-147">`.ExternalHelp` `<XML Help File>` Specifies the path and/or name of an XML-based Help file for the script or function.</span></span>

<span data-ttu-id="1e763-148">Die `.ExternalHelp` -Schlüsselwort teilt dem [Microsoft.PowerShell.Commands.Get-Help](/dotnet/api/Microsoft.PowerShell.Commands.Get-Help) Cmdlet zum Abrufen von Hilfe für das Skript oder eine Funktion in eine XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="1e763-148">The `.ExternalHelp` keyword tells the [Microsoft.PowerShell.Commands.Get-Help](/dotnet/api/Microsoft.PowerShell.Commands.Get-Help) cmdlet to get help for the script or function in an XML-based file.</span></span> <span data-ttu-id="1e763-149">Die **. ExternalHelp** -Schlüsselwort ist erforderlich, wenn eine XML-basierte Hilfedatei für ein Skript oder eine Funktion verwenden.</span><span class="sxs-lookup"><span data-stu-id="1e763-149">The **.ExternalHelp** keyword is required when using an XML-based help file for a script or function.</span></span> <span data-ttu-id="1e763-150">Ohne diese `Get-Help` eine Hilfedatei für die Funktion bzw. das Skript wird nicht gefunden.</span><span class="sxs-lookup"><span data-stu-id="1e763-150">Without it, `Get-Help` will not find a help file for the function or script.</span></span>

<span data-ttu-id="1e763-151">Die `.ExternalHelp` Schlüsselwort hat Vorrang vor allen anderen kommentarbasierte Hilfe-Schlüsselwörter.</span><span class="sxs-lookup"><span data-stu-id="1e763-151">The `.ExternalHelp` keyword takes precedence over all other comment-based help keywords.</span></span> <span data-ttu-id="1e763-152">Wenn `.ExternalHelp` vorhanden ist, die [Microsoft.PowerShell.Commands.Get-Help](/dotnet/api/Microsoft.PowerShell.Commands.Get-Help) Cmdlet zeigt keine kommentarbasierte Hilfe, selbst wenn er eine Hilfedatei, die den Wert des Schlüsselworts entspricht nicht finden kann.</span><span class="sxs-lookup"><span data-stu-id="1e763-152">When `.ExternalHelp` is present, the [Microsoft.PowerShell.Commands.Get-Help](/dotnet/api/Microsoft.PowerShell.Commands.Get-Help) cmdlet does not display comment-based help, even when it cannot find a help file that matches the value of the keyword.</span></span>

<span data-ttu-id="1e763-153">Wenn die Funktion wird durch ein Skriptmodul, den Wert des exportiert `.ExternalHelp` sollte ein Dateinamen ohne Pfad sein.</span><span class="sxs-lookup"><span data-stu-id="1e763-153">When the function is exported by a script module, the value of `.ExternalHelp` should be a file name without a path.</span></span> <span data-ttu-id="1e763-154">`Get-Help` Sucht nach der Datei in einem gebietsschemaspezifischen Unterverzeichnis des Modulverzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="1e763-154">`Get-Help` looks for the file in a locale-specific subdirectory of the module directory.</span></span> <span data-ttu-id="1e763-155">Es gibt keine Anforderungen für den Dateinamen, aber eine bewährte Methode ist, verwenden Sie das folgende Namensformat für die Datei: `<ScriptModule>.psm1-help.xml`.</span><span class="sxs-lookup"><span data-stu-id="1e763-155">There are no requirements for the file name, but a best practice is to use the following file name format: `<ScriptModule>.psm1-help.xml`.</span></span>

<span data-ttu-id="1e763-156">Wenn die Funktion keinem Modul zugeordnet ist, schließen Sie einen Pfad und Dateinamen ein, in den Wert des der **. ExternalHelp** Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="1e763-156">When the function is not associated with a module, include a path and file name in the value of the **.ExternalHelp** keyword.</span></span> <span data-ttu-id="1e763-157">Wenn der angegebene Pfad der XML-Datei UI-kulturspezifischen Unterverzeichnissen enthält `Get-Help` durchsucht die Unterverzeichnisse rekursiv für eine XML-Datei durch den Namen des Skripts bzw. einer Funktion in Übereinstimmung mit der Sprache, die fallback-Standards für eingerichtet Windows, wird genau wie für alle XML-basierte Hilfethemen.</span><span class="sxs-lookup"><span data-stu-id="1e763-157">If the specified path to the XML file contains UI-culture-specific subdirectories, `Get-Help` searches the subdirectories recursively for an XML file with the name of the script or function in accordance with the language fallback standards established for Windows, just as it does for all XML-based Help topics.</span></span>

<span data-ttu-id="1e763-158">Weitere Informationen zu den Cmdlet-Hilfe XML-basierte Hilfedatei-Format, finden Sie unter [Schreiben von Windows PowerShell-Cmdlet-Hilfe](./writing-help-for-windows-powershell-cmdlets.md).</span><span class="sxs-lookup"><span data-stu-id="1e763-158">For more information about the cmdlet Help XML-based Help file format, see [Writing Windows PowerShell Cmdlet Help](./writing-help-for-windows-powershell-cmdlets.md).</span></span>