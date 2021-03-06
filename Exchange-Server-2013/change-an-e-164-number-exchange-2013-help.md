﻿---
title: 'Modificare un numero e. 164: Exchange 2013 Help'
TOCTitle: Modificare un numero e. 164
ms:assetid: 2a3da11b-bb9b-4d4d-9238-6a1a47ef63f2
ms:mtpsurl: https://technet.microsoft.com/it-it/library/Dd335162(v=EXCHG.150)
ms:contentKeyID: 50555560
ms.date: 05/22/2018
mtps_version: v=EXCHG.150
ms.translationtype: MT
---

# Modificare un numero e. 164

 

_**Si applica a:** Exchange Online, Exchange Server 2013, Exchange Server 2016_

_**Ultima modifica dell'argomento:** 2012-11-14_

Quando si abilita un utente alla messaggistica unificata e lo si collega ad un dial plan E.164, vengono creati due indirizzi proxy di messaggistica unificata di Exchange. Uno contiene il numero di interno dell'utente e l'altro contiene il numero E.164 per l'utente. Il numero di interno viene utilizzato quando l'utente chiama un numero di Outlook Voice Access.

È possibile modificare il numero primario E.164 che è stato aggiunto quando l'utente era abilitato alla messaggistica unificata o un numero secondario E.164 aggiunto successivamente, insieme agli indirizzi proxy di messaggistica unificata per l'utente. Il numero E.164 primario aggiunto quando l'utente era abilitato alla messaggistica unificata verrà riportato come indirizzo proxy primario della messaggistica unificata di Exchange. Qualsiasi altro numero secondario E.164 che si aggiunge verrà riportato come indirizzo proxy secondario di messaggistica unificata di Exchange. Quando i numeri E.164 saranno stati modificati, i chiamanti potranno lasciare messaggi vocali per l'utente a tutti i nuovi numeri E.164 che sono stati impostati. Tutti i messaggi vocali verranno inoltrati alla cassetta postale dello stesso utente.

È possibile utilizzare l'interfaccia di amministrazione di Exchange o Shell per modificare i numeri E.164 primari e secondari per un utente. È possibile utilizzare la pagina **Indirizzo di posta elettronica** sulla cassetta postale dell'utente per modificare un numero E.164 primario o secondario. Tuttavia, non è possibile utilizzare la pagina **Cassetta postale di messaggistica unificata** nell'interfaccia di amministrazione di Exchange per modificare un numero E.164 primario o secondario.

È possibile visualizzare i numeri E.164 primari e secondari per un utente utilizzando il cmdlet **Get-UMMailbox** o il cmdlet **Get-Mailbox** in Shell.

Per altre attività di gestione relative agli utenti abilitati alla posta vocale, vedere [Procedure utente abilitato alla posta vocale](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## Che cosa è necessario sapere prima di iniziare?

  - Tempo stimato per il completamento: 3 minuti.

  - Per eseguire queste procedure, è necessario disporre delle autorizzazioni appropriate. Per sapere quali autorizzazioni sono necessarie, vedere "Cassette postali di messaggistica unificata" nell'argomento [Autorizzazioni della messaggistica unificate](unified-messaging-permissions-exchange-2013-help.md).

  - È necessario confermare la creazione del dial plan di messaggistica unificata E.164 prima di eseguire le procedure. Per la procedura dettagliata, vedere [Creazione di un dial plan di messaggistica unificata](create-a-um-dial-plan-exchange-2013-help.md).

  - Prima di eseguire queste procedure, verificare che sia stato creato un criterio di cassetta postale di messaggistica unificata. Per la procedura dettagliata, vedere [Creazione di un criterio cassetta postale di messaggistica unificata](create-a-um-mailbox-policy-exchange-2013-help.md).

  - Prima di eseguire queste procedure, confermare che la cassetta postale dell'utente sia stata abilitata per la messaggistica unificata e collegata ad un dial plan E.164. Per la procedura dettagliata, vedere [Consentire a un utente per la segreteria telefonica](enable-a-user-for-voice-mail-exchange-2013-help.md).

  - Prima di eseguire queste procedure, confermare che il numero E.164 che verrà assegnato all'utente abilitato alla messaggistica unificata sia valido.

  - Per informazioni sui tasti di scelta rapida che è possibile utilizzare con le procedure in questo argomento, vedere [Tasti di scelta rapida nell'interfaccia di amministrazione di Exchange](keyboard-shortcuts-in-the-exchange-admin-center-exchange-online-protection-help.md).


> [!TIP]
> Problemi? È possibile richiedere supporto nei forum di Exchange. I forum sono disponibili sui seguenti siti: <A href="https://go.microsoft.com/fwlink/p/?linkid=60612">Exchange Server</A>, <A href="https://go.microsoft.com/fwlink/p/?linkid=267542">Exchange Online</A> o <A href="https://go.microsoft.com/fwlink/p/?linkid=285351">Exchange Online Protection</A>..



## Operazione desiderata?

## Utilizzo dell'interfaccia di amministrazione di Exchange per modificare un numero E.164 primario o secondario

1.  In Interfaccia di amministrazione di Exchange accedere a **Destinatari** \> **Cassette postali**.

2.  Nella visualizzazione elenco, selezionare la cassetta postale per cui si desidera modificare un numero E.164, quindi fare clic su **Modifica**![Icona Modifica](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Icona Modifica").

3.  Nella pagina **Cassetta postale utente**, sotto **Indirizzo di posta elettronica**, selezionare il numero E.164 che si desidera modificare, quindi fare clic su **Modifica**![Icona Modifica](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Icona Modifica"). Il numero primario E.164 viene riportato in lettere e numeri in grassetto.

4.  Nella pagina **Indirizzo di posta elettronica**, nella casella **Indirizzo/Estensione**, immettere il nuovo numero E.164 per l'utente, quindi fare clic su **OK**. Per selezionare un nuovo dial plan di messaggistica unificata, fare clic su **Sfoglia**.

5.  Fare clic su **Salva**.

## Utilizzo di Shell per modificare il numero E.164 primario o secondario

In questo esempio viene modificato un numero E.164 per Tony Smith, un utente abilitato alla messaggistica unificata.


> [!NOTE]
> Prima di modificare un numero E.164 utilizzando Shell, è necessario determinare la posizione dell'indirizzo proxy di messaggistica unificata di Exchange che si desidera modificare. Per determinare la posizione, utilizzare il comando <STRONG>$mbx.EmailAddresses</STRONG>. Il primo indirizzo proxy di messaggistica unificata di Exchange è il numero E.164 (primario) predefinito e sarà 0 nell'elenco.



    $mbx=Get-Mailbox tony.smith
    $mbx.EmailAddresses.Item(1)="eum:+14255550123;phone-context=MyE.164DialPlan.contoso.com"
    Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses

