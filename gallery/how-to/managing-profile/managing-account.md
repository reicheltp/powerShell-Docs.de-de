---
ms.date: 09/05/2018
contributor: JKeithB
keywords: gallery,powershell,cmdlet,psgallery
title: Kontoeinstellungen für den PowerShell-Katalog
ms.openlocfilehash: dd7b8b3a99b5b7fbfec5d7f82b81dd6d5cfb906c
ms.sourcegitcommit: e46b868f56f359909ff7c8230b1d1770935cce0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45523136"
---
# <a name="powershell-gallery-account-settings"></a><span data-ttu-id="e2556-103">Kontoeinstellungen für den PowerShell-Katalog</span><span class="sxs-lookup"><span data-stu-id="e2556-103">PowerShell Gallery Account Settings</span></span>

<span data-ttu-id="e2556-104">Ihr Konto für den PowerShell-Katalog ist ein öffentlich sichtbarer Name, der mit einer Identität verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="e2556-104">Your PowerShell Gallery account is a publicly visible name that is linked to an identity.</span></span> <span data-ttu-id="e2556-105">Diese Identität ist entweder eine Microsoft-ID, wie z.B. `user@hotmail.com` oder `user@outlook.com`, oder ein AAD-Konto.</span><span class="sxs-lookup"><span data-stu-id="e2556-105">That identity is either a Microsoft ID, like `user@hotmail.com` or `user@outlook.com`, or an Azure Active Directory (AAD) account.</span></span>

<span data-ttu-id="e2556-106">Über den PowerShell-Katalog können Sie die folgenden Einstellungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="e2556-106">The PowerShell Gallery provides the following account settings:</span></span>

- <span data-ttu-id="e2556-107">Sie können das E-Mail-Konto festlegen, das mit Ihrem PowerShell-Katalog-Konto verknüpft sein soll.</span><span class="sxs-lookup"><span data-stu-id="e2556-107">The email account associated with your PowerShell Gallery account</span></span>
- <span data-ttu-id="e2556-108">Sie können die E-Mail-Benachrichtigungseinstellungen anpassen, die vom PowerShell-Katalog gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="e2556-108">Options for email notifications sent from the PowerShell Gallery</span></span>
- <span data-ttu-id="e2556-109">Sie können das Benutzerkonto festlegen, das mit Ihrem PowerShell-Katalog-Konto verknüpft sein soll.</span><span class="sxs-lookup"><span data-stu-id="e2556-109">The user account associated with your PowerShell Gallery account</span></span>
- <span data-ttu-id="e2556-110">Sie können das Profilbild festlegen, das mit Ihrem PowerShell-Katalog-Konto verknüpft sein soll.</span><span class="sxs-lookup"><span data-stu-id="e2556-110">The picture associated with your PowerShell Gallery account</span></span>

## <a name="email-address"></a><span data-ttu-id="e2556-111">E-Mail-Adresse</span><span class="sxs-lookup"><span data-stu-id="e2556-111">Email address</span></span>

<span data-ttu-id="e2556-112">An die E-Mail-Adresse werden die Benachrichtigungen des PowerShell-Katalogs gesendet.</span><span class="sxs-lookup"><span data-stu-id="e2556-112">The email address is the destination for PowerShell Gallery notifications.</span></span> <span data-ttu-id="e2556-113">Sie muss nicht mit dem Anmeldekonto übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="e2556-113">It does not have to match the login account.</span></span> <span data-ttu-id="e2556-114">Sie können eine beliebige Ihrer E-Mail-Adressen verwenden.</span><span class="sxs-lookup"><span data-stu-id="e2556-114">You may use any email account you have access to.</span></span> <span data-ttu-id="e2556-115">Ihre E-Mail-Adresse wird nicht direkt an andere Benutzer des PowerShell-Katalogs weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="e2556-115">Your email address is never directly provided by the PowerShell Gallery to other users.</span></span>

![E-Mail-Adresse ändern](../../Images/PSGallery_AcccountEmailAddress.png)

<span data-ttu-id="e2556-117">Wenn Sie eine neue E-Mail-Adresse eingeben, sendet der PowerShell-Katalog eine Verifizierungs-E-Mail an diese Adresse.</span><span class="sxs-lookup"><span data-stu-id="e2556-117">When you enter a new email address, the PowerShell Gallery sends a verification mail to that address.</span></span> <span data-ttu-id="e2556-118">Die Verifizierungs-E-Mail enthält einen Link zurück zum PowerShell-Katalog, wo Sie den Vorgang abschließen können.</span><span class="sxs-lookup"><span data-stu-id="e2556-118">The verification mail contains a link back to the PowerShell Gallery to complete the change process.</span></span> <span data-ttu-id="e2556-119">Bis zum Abschluss der Verifizierung werden alle Benachrichtigungen an die alte E-Mail-Adresse geschickt.</span><span class="sxs-lookup"><span data-stu-id="e2556-119">Until you complete the verification process, all notifications are sent to the previous address.</span></span>

## <a name="email-notifications"></a><span data-ttu-id="e2556-120">E-Mail-Benachrichtigungen</span><span class="sxs-lookup"><span data-stu-id="e2556-120">Email notifications</span></span>

<span data-ttu-id="e2556-121">Über den PowerShell-Katalog können Sie die folgenden Benachrichtigungseinstellungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="e2556-121">PowerShell Gallery provides the following notification options:</span></span>

- <span data-ttu-id="e2556-122">Benutzer können mich über den PowerShell-Katalog kontaktieren</span><span class="sxs-lookup"><span data-stu-id="e2556-122">Users can contact me through the PowerShell Gallery</span></span>
- <span data-ttu-id="e2556-123">Benachrichtigen, wenn ein Element mit meinem Konto in den PowerShell-Katalog gepusht wird</span><span class="sxs-lookup"><span data-stu-id="e2556-123">Notify me when an item is pushed to the PowerShell Gallery using my account</span></span>

![E-Mail-Adresse ändern](../../Images/PSGallery_AccountEmailOptions.png)

<span data-ttu-id="e2556-125">Wie auch auf der Seite angemerkt, können wichtige Benachrichtigungen des PowerShell-Katalogs nicht deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="e2556-125">As noted on the page, critical notifications from the PowerShell Gallery can't be disabled.</span></span>
<span data-ttu-id="e2556-126">Dazu zählen:</span><span class="sxs-lookup"><span data-stu-id="e2556-126">These include:</span></span>

- <span data-ttu-id="e2556-127">Sicherheitsbenachrichtigungen</span><span class="sxs-lookup"><span data-stu-id="e2556-127">Security notifications</span></span>
- <span data-ttu-id="e2556-128">Benachrichtigungen zur Kontoverwaltung von PowerShell-Katalog-Administratoren</span><span class="sxs-lookup"><span data-stu-id="e2556-128">Account management notifications from PowerShell Gallery administrators</span></span>
- <span data-ttu-id="e2556-129">Benachrichtigungen zu vom PowerShell-Katalog ausgeführten Tests bezüglich Paketen, die Sie eingereicht haben</span><span class="sxs-lookup"><span data-stu-id="e2556-129">Notifications about the tests run by the PowerShell Gallery for submissions you have made</span></span>

## <a name="change-your-login-account"></a><span data-ttu-id="e2556-130">Ändern des Anmeldekontos</span><span class="sxs-lookup"><span data-stu-id="e2556-130">Change your login account</span></span>

<span data-ttu-id="e2556-131">Sie müssen mit dem aktuellen Konto angemeldet sein, um das Anmeldekonto zu ändern.</span><span class="sxs-lookup"><span data-stu-id="e2556-131">To change the login account, you must be signed in with the current account.</span></span> <span data-ttu-id="e2556-132">Führen Sie zum Ändern die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="e2556-132">Use the following steps to complete the change.</span></span>

![Kontoeinstellungen](../../Images/PSGallery_LoginAccountSettings.png)

1. <span data-ttu-id="e2556-134">Klicken Sie auf **Kontotyp ändern**.</span><span class="sxs-lookup"><span data-stu-id="e2556-134">Click on **Change Account**.</span></span> <span data-ttu-id="e2556-135">In einem Popupfenster wird erklärt, dass die Änderung des Anmeldekontos für den gesamten PowerShell-Katalog gilt.</span><span class="sxs-lookup"><span data-stu-id="e2556-135">A pop-up window explains that changing the login account applies to all uses of that account in the PowerShell Gallery.</span></span> <span data-ttu-id="e2556-136">Überprüfen Sie die Informationen, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2556-136">Review the information, then click **OK** to continue.</span></span>

   ![Kontoeinstellungen](../../Images/PSGallery_LoginAccountChange-1.png)

2. <span data-ttu-id="e2556-138">Dann werden Sie aufgefordert, sich mit dem _neuen Konto_ anzumelden.</span><span class="sxs-lookup"><span data-stu-id="e2556-138">You are then prompted to sign in using the _new account_.</span></span>

   ![Kontoeinstellungen](../../Images/PSGallery_LoginAccountChange-2.png)

3. <span data-ttu-id="e2556-140">Wenn Sie auf **Weiter klicken**, wird eine Meldung angezeigt, dass Sie noch mit Ihrem alten Konto angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="e2556-140">When you click **Next**, you see a message that you are signed in using the current account.</span></span>
   <span data-ttu-id="e2556-141">Klicken Sie auf **Sign out and sign in with a different account** (Abmelden und mit anderem Konto anmelden).</span><span class="sxs-lookup"><span data-stu-id="e2556-141">Click **Sign out and sign in with a different account**.</span></span>

   ![Kontoeinstellungen](../../Images/PSGallery_LoginAccountChange-3.png)

4. <span data-ttu-id="e2556-143">Geben Sie das Kennwort für das neue Konto ein.</span><span class="sxs-lookup"><span data-stu-id="e2556-143">Enter the password of the new account.</span></span> <span data-ttu-id="e2556-144">Wenn Sie das Kennwort eingegeben haben, werden Sie zu den Kontoeinstellungen weitergeleitet. Dort wird angezeigt, dass das Anmeldekonto aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="e2556-144">After entering the password, you are returned to the Account Settings page showing you that the login account has been updated.</span></span>


## <a name="enable-two-factor-authentication-2fa"></a><span data-ttu-id="e2556-145">Aktivieren der zweistufigen Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="e2556-145">Enable Two-Factor Authentication (2FA)</span></span>

<span data-ttu-id="e2556-146">Die zweistufige Authentifizierung wird für alle Benutzer empfohlen, die ihre Veröffentlichungen im PowerShell-Katalog manuell durchführen.</span><span class="sxs-lookup"><span data-stu-id="e2556-146">Two-factor authentication is recommended for all users who publish manually to the PowerShell Gallery.</span></span> <span data-ttu-id="e2556-147">Für das Anmeldekonto müssen mindestens zwei Authentifizierungsmethoden registriert sein, damit die zweistufige Authentifizierung aktiviert werden kann.</span><span class="sxs-lookup"><span data-stu-id="e2556-147">To enable two-factor authentication, the login account must have at least two forms of authentication registered.</span></span> <span data-ttu-id="e2556-148">Die eine Methode ist ein Kennwort, und die andere kann eine der folgenden Methoden sein:</span><span class="sxs-lookup"><span data-stu-id="e2556-148">One is a password and the other forms can be:</span></span>

- <span data-ttu-id="e2556-149">eine Telefonnummer, an die SMS gesendet werden können</span><span class="sxs-lookup"><span data-stu-id="e2556-149">A phone number that can receive text messages</span></span>
- <span data-ttu-id="e2556-150">eine registrierte Authentifikatoranwendung, wie z.B. Microsoft Authenticator, für Ihr Mobiltelefon</span><span class="sxs-lookup"><span data-stu-id="e2556-150">A registered authenticator application, such as Microsoft Authenticator for your mobile phone</span></span>

<span data-ttu-id="e2556-151">Die Authentifizierungsmethoden müssen in Ihren AAD-Kontoinformationen oder in den Kontosicherheitseinstellungen Ihrer Microsoft-ID konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="e2556-151">These forms of authentication must be configured in your AAD account information or in your Microsoft ID Account Security settings.</span></span>

<span data-ttu-id="e2556-152">Wenn Sie die zweistufige Authentifizierung aktivieren, müssen Sie sich bei jeder Anmeldung beim PowerShell-Katalog mit den konfigurierten Methoden authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="e2556-152">Once 2FA is enabled, you are required to authenticate using the configured forms of authentication every time you sign in to the PowerShell Gallery.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2556-153">Sie müssen die zweistufige Authentifizierung nicht für alle Benutzer des Anmeldekontos aktivieren, wenn Sie sie für den PowerShell-Katalog aktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="e2556-153">Enabling two-factor authentication for the PowerShell Gallery site does not require you to enable 2FA for all uses of your login account.</span></span> <span data-ttu-id="e2556-154">Weitere Informationen finden Sie unter [Informationen zur Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span><span class="sxs-lookup"><span data-stu-id="e2556-154">For more information, see [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span></span>

## <a name="change-your-profile-picture"></a><span data-ttu-id="e2556-155">Ändern des Profilbilds</span><span class="sxs-lookup"><span data-stu-id="e2556-155">Change your profile picture</span></span>

<span data-ttu-id="e2556-156">Gravatar ist im PowerShell-Katalog für das Speichern und Anzeigen von Profilbildern zuständig.</span><span class="sxs-lookup"><span data-stu-id="e2556-156">The PowerShell Gallery relies on Gravatar to store and display the picture associated with your profile.</span></span> <span data-ttu-id="e2556-157">Besuchen Sie [gravatar.com](http://www.gravatar.com/), um Ihr Profilbild zu ändern.</span><span class="sxs-lookup"><span data-stu-id="e2556-157">To update your profile image, visit [Gravatar.com](http://www.gravatar.com/).</span></span>