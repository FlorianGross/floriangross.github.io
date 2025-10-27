# Windows Server 2025 – Fehler „Online – Fehler beim Dateiabruf“ im Server-Manager beheben

## Überblick

In Windows Server 2025 kann es bei der Verwaltung von Remote-Systemen über den **Server-Manager** zu folgender Fehlermeldung kommen:

> **Online – Fehler beim Dateiabruf**

Dieser Fehler tritt häufig auf, wenn der **Windows Remote Management (WinRM)**-Dienst auf dem Zielsystem bestimmte Anforderungsgrößen überschreitet oder die Standardkonfiguration zu restriktiv ist. Das Resultat: Der Server-Manager kann keine vollständigen Systemdaten abrufen.

---

## Ursache

Der Server-Manager verwendet für die Kommunikation mit Remote-Systemen das **WinRM-Protokoll** (Windows Remote Management).  
Standardmäßig ist im WinRM-Stack eine maximale Paketgröße (`MaxEnvelopeSizekb`) von **500 KB** definiert. Bei Systemen mit vielen installierten Rollen, Features oder umfangreichen WMI-Antworten kann dieser Wert zu niedrig sein – der Server-Manager bricht den Abruf dann mit dem genannten Fehler ab.

---

## Lösung

Der Fehler lässt sich beheben, indem der zulässige Envelope-Size-Wert auf dem **Zielsystem** erhöht wird.  
Dies geschieht über den folgenden PowerShell-Befehl, der **mit Administratorrechten** ausgeführt werden muss:

```powershell
Set-WSManInstance -ResourceURI winrm/config -ValueSet @{MaxEnvelopeSizekb = "8192"}
