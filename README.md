# Splunk Technology Add-on (TA) for Azure Entra ID (AD) Mailbox Settings
Ah, the endless, soul-draining quest for clarity in identity management. You’d think, in this age of infinite computing power, that figuring out whether an account belongs to a living, breathing human being or some nameless, faceless service mailbox would be trivial. But no. Instead, we scrape, we cross-reference, we build "master lookup tables" in Splunk, and still, we find ourselves staring at data that refuses to tell us anything useful.

If you're in Azure Entra ID (née Azure AD, because nothing gold can stay), you might be using TA-MS-AAD or the Splunk Add-on for Microsoft Azure, grabbing `sourcetype=azure:aad:user`. And sure, the Graph API endpoint gives you a heap of data—especially if you embrace the chaos of the beta version—but good luck untangling whether an entry represents an actual person or just another shared mailbox, doomed to exist without agency or identity.

Now, if your organization has a clear, rational way of tagging OU in `distinguishedName`, then congratulations, you’ve been spared this particular absurdity. But if not? Well, prepare to drown in a sea of indistinguishable accounts, where every email address is Schrödinger’s user—both real and not, until observed.

Microsoft Graph does, at least, offer `mailboxSettings` [here](https://learn.microsoft.com/en-us/graph/api/resources/mailboxsettings), which contains the `userPurpose` field—the closest thing to an answer you’re going to get. See here if you feel like chasing meaning in the void.

To save you from existential despair, I built this collector. It pulls the `userPurpose` field and maps it to your Identities using either `id` or `userPrincipalName`. Will it solve all your problems? No. But it might, at least, help you make sense of this ridiculous game we call IT.

_(I had ChatGPT rewrite my TA description to sound like a grumpy Albert Camus 🤪)_

## Requirements
- An Azure app-registration (that's your Client ID)
- This Client ID must have `MailboxSettings.Read` permission

## How to make it work
- Install this on your HF or SplunkCloud stack
- Create an input stanza
- Give it a name
- Give it a very long interval (you don't need this that much, once a week is fine: 604800)
- Enter your Client ID, Client Secret, and the Azure Directory Tenant ID
- Enter the index you wish to dump these events (they're not events, btw. just a dump of user accounts with specific information)

## Sample log

```
{
   "id": "6ef43087-2aa7-462d-bb7d-5f6841296f3f",
   "mail": "musk.zuckergates.communications@faang.com",
   "mailboxSettingsUserPurpose": "shared",
   "userPrincipalName": "musk.zuckergates.communications@faang.com"
}
```
---
### For the IPA
Paypal: daniel.l.astillero@gmail.com