# Lab 3 - PostgreSQL nach Azure migrieren.

### Zielsetzung

Im diesem Lab werden wir eine virtuelle Maschine bereitstellen, um die
**PostgreSQL- Datenbank** zu hosten und die erforderliche **PostgreSQL
infrastructure** zu erstellen. Anschließend werden wir die PostgreSQL
Database mithilfe des **Azure Database for Postgres Flexible Server
(Migration)** migrieren**.** 

![](./media-DE/image1.png)

### Aufgabe 1 - Bereitstellung der virtuellen Maschine zum Hosten der PostgreSQL-Datenbank für die On-Premises-Umgebung.
Wir werden **Ubuntu 22.0.4.4 LTS** VM einsetzen, auf der wir
**PostgreSQL Server 16** installieren und dann die Beispieldatenbank
erstellen, die für die Migration verwendet wird.

1.  Öffnen Sie über das Azure Portal `https://portal.azure.com` die
    Azure Cloud Shell

    ![A screenshot of a computer Description automatically
generated](./media-DE/image2.png)

2.  Klicken Sie auf die Schaltfläche **PowerShell**.

    ![A screenshot of a computer error Description automatically
generated](./media-DE/image3.png)

3.  Wählen Sie im Fenster **Getting started** die Optionsschaltfläche
    für **Mount storage account** und dann das **Azure Pass –
    Sponsorship** subscription aus und klicken Sie auf die Schaltfläche
    **Apply** .

    ![A screenshot of a computer Description automatically
generated](./media-DE/image4.png)

4.  Wählen Sie im Fenster Mount storage account das Optionsfeld **We
    will create storage account for you** und klicken Sie dann auf
    **Next**.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image5.png)

5.  Warten auf den Abschluss des Deployments

    ![A screenshot of a computer program Description automatically
generated](./media-DE/image6.png)

6.  Geben Sie im Cloud Shell PowerShell -Fenster die folgenden Befehle
    ein, um die Variablen zu konfigurieren und die VM zu erstellen, die
    für die Installation des PostgreSQL-Servers verwendet werden soll.

    `$cred = Get-Credential`

7.  Wenn Sie aufgefordert werden, Ihre Anmeldedaten einzugeben, geben
    Sie Folgendes ein

    User - `postgres`

    Password - `P@55w.rd1234`

    ![A screenshot of a computer screen Description automatically
generated](./media-DE/image7.png)

8.  Geben Sie den folgenden Befehl ein, um die Ressourcengruppe zu
    erstellen

    `New-AzResourceGroup -ResourceGroupName "PostgresRG" -Location "WestUS"`

    ![](./media-DE/image8.png)

9.  Geben Sie den folgenden Befehl ein, um die Windows Server 2019
    Datacenter VM bereitzustellen

    ` New-AzVm \`

    -ResourceGroupName "PostgresRG" \`

    -Name "PostgresSrv" \`

    -Location "WestUS" \`

    -VirtualNetworkName "PGVnet" \`

    -SubnetName "PGSubnet" \`

    -SecurityGroupName "PostgresNSG" \`

    -Securitytype "Standard" \`

    -PublicIpAddressName "PostgresSrvIP" \`

    -ImageName
    "Canonical:0001-com-ubuntu-server-jammy:22_04-lts-gen2:latest" \`

    -Credential $cred \`

    -Size "Standard_b2ms"`

    ![A computer screen shot of a computer Description automatically
generated](./media-DE/image9.png)

10. Sobald das Deployment abgeschlossen ist, wird die folgende Meldung
    angezeigt

    ![A blue screen with white text Description automatically
generated](./media-DE/image10.png)

11. Führen Sie den folgenden Befehl aus, um eine Verbindung mit der
    Ubuntu-VM herzustellen. Ersetzen Sie den Befehl durch den
    **FullyQualifiedDomainName** aus der Ausgabe des vorherigen Befehls

    ![A screen shot of a computer Description automatically
generated](./media-DE/image11.png)

    `ssh postgres@FullyQualifiedDomainName`

    ![](./media-DE/image12.png)

12. Wenn Sie aufgefordert werden, fortzufahren, geben Sie **"Yes"** und
    dann das während des Deployments angegebene Passwort ein -
    `P@55w.rd1234`

13. Es sollte sich erfolgreich mit dem Ubuntu Server verbinden

    ![](./media-DE/image13.png)

14. Jetzt installieren wir **PostgreSQL ver. 16** auf der Ubuntu VM,
    werden wir die automatische Repository-Konfiguration einstellen,
    indem wir den folgenden Befehl ausführen

    `sudo apt install -y postgresql-common`

    `sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh `

    !](./media-DE/image14.png)

    ![](./media-DE/image15.png)

15. Drücken Sie die Eingabetaste auf der Tastatur, um fortzufahren.

    ![](./media-DE/image16.png)

    ![](./media-DE/image17.png)

16. Wir **importieren den Repository Signing Key**, indem wir die
    folgenden Befehle ausführen.

    `sudo apt install curl ca-certificates`

    `sudo install -d /usr/share/postgresql-common/pgdg`

    `sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc
    --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc`

    ![](./media-DE/image18.png)

17. Wir führen den folgenden Befehl aus, um **die repository
    configuration file zu erstellen**

    `sudo apt update`

    `sudo apt install gnupg2 wget`

    `sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt
    $(lsb_release -cs)-pgdg main" \ /etc/apt/sources.list.d/pgdg.list'`

    `curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo
    gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg`

    ![](./media-DE/image19.png)

18. Wir werden den folgenden Befehl ausführen, um **die package lists zu
    aktualisieren**

    `sudo apt update`

    ![A computer screen with white text Description automatically
generated](./media-DE/image20.png)

19. Wir werden den folgenden Befehl ausführen, um **die neueste Version
    von PostgreSQL zu installieren**

    `sudo apt install postgresql-16 postgresql-contrib-16`

    ![A computer screen shot of a blue screen Description automatically
generated](./media-DE/image21.png)

    > **Hinweis - Die Installation sollte in 1-2 Minuten abgeschlossen sein.**

    ![A blue screen with white text Description automatically
generated](./media-DE/image22.png)

    ![A blue screen with white text Description automatically
generated](./media-DE/image23.png)

20. Sobald die Installation abgeschlossen ist, geben Sie den folgenden
    Befehl ein, um das PSQL-Dienstprogramm zu starten

    `Psql`

    ![A blue screen with white text Description automatically
generated](./media-DE/image24.png)

21. Wir werden das Passwort für das **postgres**-Konto in psql
    festlegen

    `\passwort postgres`

22. Geben Sie das Passwort als `postgres` ein. Geben Sie es erneut als

    `postgres`

    ![A computer screen shot of a blue screen Description automatically
generated](./media-DE/image25.png)

23. Jetzt werden wir die Netzwerk- und andere Berechtigungen für alle
    PostgreSQL Server setzen, auf die aus der Ferne zugegriffen werden
    soll

24. Führen Sie den folgenden Befehl aus, um auf die Datei
    **postgresql.conf** zuzugreifen

    `\q`

    `sudo nano /etc/postgresql/16/main/postgresql.conf`

25. Sobald die Datei geöffnet ist, scrollen Sie nach unten und
    aktualisieren Sie die Einstellungen wie folgt

    > **Entfernen Sie unter Connection Settings das \# und ändern Sie
    listen_addresses = '\*'.**

    ![A computer screen with white text Description automatically
generated](./media-DE/image26.png)

    > **Unter WRITE-AHEAD LOG entfernen Sie das \# und ändern wal_level =
    logical**

    ![A computer screen with white text Description automatically
generated](./media-DE/image27.png)

26. Sobald die obige Änderung erfolgt ist, drücken Sie **Ctrl + X**

    ![A screenshot of a computer program Description automatically
generated](./media-DE/image28.png)

27. Drücken Sie zur Bestätigung die Taste **Y** und die Eingabetaste.

28. Führen Sie den folgenden Befehl aus, um auf die Datei
    **pg_hba.conf** zuzugreifen

    `sudo nano /etc/postgresql/16/main/pg_hba.conf`

29. Sobald die Datei geöffnet ist, scrollen Sie nach unten und fügen Sie
    die folgenden Zeilen am Ende der Datei hinzu


    `host all all 0.0.0.0/0 md5`

    `host all all ::/0 md5`

    ![A screenshot of a computer Description automatically
generated](./media-DE/image29.png)

30. Sobald die obige Änderung erfolgt ist, drücken Sie **Ctrl + X**

    ![A blue screen with white text Description automatically
generated](./media-DE/image30.png)

31. Drücken Sie zur Bestätigung die Taste **Y** und die Eingabetaste.

32. Führen Sie den folgenden Befehl aus, um den PostgreSQL-Dienst neu zu
    starten

    `sudo service postgresql restart`

    ![A blue screen with white text Description automatically
generated](./media-DE/image31.png)

33. Suchen Sie im Azure Portal und wählen Sie !!Resource groups!!.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image32.png)

34. Wählen Sie in der Liste der Resource groups **PostgresRG** und dann
    die VM - **PostgresSrv**

35. Wählen Sie auf der Seite **PostgresSrv** die **Networking
    setting** und klicken Sie dann auf **+ Create port rule** und wählen
    Sie **Inbound port Rule**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image33.png)

36. Wählen Sie auf der Seite **Add inbound security rule** unter service
    aus der Dropdown-Liste **PostgreSQL** aus und klicken Sie dann auf
    die Schaltfläche **Add**.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image34.png)

37. Sie sollten eine Benachrichtigung erhalten, wie in der folgenden
    Abbildung gezeigt.

    ![A white background with blue text Description automatically
generated](./media-DE/image35.png)

38. Nun ist der PostgreSQL-Server für den Fernzugriff bereit.

### Aufgabe 2 - Erstellen Sie eine PostgreSQL-Datenbank für die On-premises Umgebung.

1.  Nun werden wir eine Beispieldatenbank in den PostgreSQL-Server
    importieren, die wir für die Migration verwenden werden

2.  Es gibt 15 Tabellen in der DVD Rental database

    ![PostgreSQL Sample Database Diagram](./media-DE/image36.png)

3.  Öffnen Sie über das Azure Portal die Azure Cloud Shell

    ![A screenshot of a computer Description automatically
generated](./media-DE/image2.png)

4.  Vergewissern Sie sich, dass die Cloud Shell mit Bash gestartet
    wurde, und führen Sie dann den folgenden Befehl aus, um sich mit der
    **PostgresSrv** VM zu verbinden

    `ssh postgres@ServerDNSName`

    ![A screenshot of a computer Description automatically
generated](./media-DE/image37.png)

5.  Wenn Sie aufgefordert werden, fortzufahren, geben Sie **"Yes"** und
    dann das Passwort ein - ` P@55w.rd1234`

6.  Es sollte sich erfolgreich mit dem Ubuntu Server verbinden

    ![A screenshot of a computer Description automatically
generated](./media-DE/image38.png)

7.  Führen Sie an der Eingabeaufforderung **postgres@PostgresSrv** den
    folgenden Befehl aus, um einen **folder** zu erstellen, in den die
    Datei kopiert wird, die für die Wiederherstellung der Datenbank
    verwendet werden soll.

    `mkdir dvdrentalbkp`

    ![](./media-DE/image39.png)

8.  Klicken Sie auf der Lab VM mit der rechten Maustaste auf das Start
    menu und wählen Sie Windows Terminal (admin)

    ![A screenshot of a computer Description automatically
generated](./media-DE/image40.png)

9.  Führen Sie im Windows PowerShell -Fenster den Befehl zum Kopieren
    der PostgreSQL-Datenbanksicherung in den Ordner **dvdrentalbkp** auf
    dem **PostgresSrv** aus.

    > **Hinweis** - Ersetzen Sie den Befehl vor der Ausführung durch den
    **FQDN Ihrer Ububtu Server VM**, bevor Sie den Befehl ausführen. siehe

    `scp "C:\Labfiles\dvdrental.tar"postgres@FQDNofUbubtuServerVM:"dvdrentalbkp"`

    Wenn Sie aufgefordert werden, fortzufahren, geben Sie **"Yes"** und dann
das Passwort ein - `P@55w.rd1234`

    ![A computer screen with white text Description automatically
generated](./media-DE/image41.png)

10. Wechseln Sie zurück auf die Registerkarte der Eingabeaufforderung
    **postgres@PostgresSrv** und führen Sie den folgenden Befehl aus, um
    PSQL zu starten

    `psql`

    ![A black screen with white text Description automatically
generated](./media-DE/image42.png)

11. Führen Sie an der **psql**-Eingabeaufforderung den folgenden Befehl
    aus, um eine Datenbank zu erstellen

    `CREATE DATABASE dvdrental;`

    ![A black screen with yellow text Description automatically
generated](./media-DE/image43.png)

    `\q`

    ![A black background with green text Description automatically
generated](./media-DE/image44.png)

12. Zurück an der Eingabeaufforderung **postgres@PostgresSrv** geben Sie
    den folgenden Befehl ein, um die Sicherung in die neu erstellte
    Datenbank zurückzuspielen.

    `cd dvdrentalbkp`

    `pg_restore -U postgres -d dvdrental "dvdrental.tar"`

    ![](./media-DE/image45.png)

    > **Hinweis**: Wenn eine Fehler- oder Warnmeldung erscheint, können Sie
    diese getrost ignorieren, und die leere Datenbank wird mit 15 Tabellen
    aktualisiert.

13. Wir können die Datenbankdetails überprüfen, indem wir die folgenden
    Befehle ausführen

    `psql

    \c dvdrental`

    ![A screen shot of a computer Description automatically
generated](./media-DE/image46.png)

    `\dt`

    ![A screenshot of a computer Description automatically
generated](./media-DE/image47.png)

### Aufgabe 3 - Erstellen einer Azure Database for PostgreSQL flexible Server

1.  Öffnen Sie den Edge-Browser und navigieren Sie zum Azure Portal
    `https://portal.azure.com`

2.  Suchen Sie nach `postgres` und wählen Sie **Azure Database for
    PostgreSQL flexible Servers**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image48.png)

3.  Klicken Sie auf **+ Create**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image49.png)

4.  Geben Sie auf der Seite **New Azure Database for PostgreSQL Flexible
    Server** auf der Registerkarte **Basics** die folgenden Details an

    - Resource group - Klicken Sie auf " Create new " und geben Sie
      einen Namen ein - `RG4AzPGDb`

    - Server name - `ad4pfssrvXXXXX ` ersetzen Sie XXXXX durch eine
      Zufallszahl

    - Region - **West US**

    - PostgreSQL version – **16**

    - Workload type – **Production**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image50.png)

    - High availability - **Disabled**

    - Authentication method – **PostgreSQL Authentication only**

    - Admin username – `postgres`

    - Password – `P@55w.rd1234`

    - Confirm password – `P@55w.rd1234`

    - Klicken Sie auf **Next: Networking \**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image51.png)

5.  Aktivieren Sie auf der Registerkarte **Networking** das
    Kontrollkästchen **Allow public access from any Azure services
    within Azure to this server** und klicken Sie auf **+ Add Client IP
    address** fügen Sie auch die **Public IP address** des
    **PostgresSrv** hinzu und klicken Sie dann auf die Schaltfläche
    **Review + create.**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image37.png)

    ![A screenshot of a computer Description automatically
generated](./media-DE/image52.png)

6.  Überprüfen Sie die Angaben und klicken Sie auf die Schaltfläche
    **Create** .

    ![A screenshot of a computer Description automatically
generated](./media-DE/image53.png)

7.  Das Bereitstellung wird beginnen.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image54.png)

    > **Hinweis**: Das Bereitstellung dauert etwa 10 Minuten.

8.  Sobald das Bereitstellung abgeschlossen ist, klicken Sie auf die
    Schaltfläche **Go to resource**.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image55.png)

### Aufgabe 4 - Migrieren Sie die PostgreSQL-database zu Azure Database for PostgreSQL flexible server (Migration)

1.  Die **Overview** Seite des **flexiblen Azure Database for PostgreSQL
    flexible server** sollte sich öffnen

    ![A screenshot of a computer Description automatically
generated](./media-DE/image56.png)

2.  Überprüfen Sie sich die overview Seite an und prüfen Sie die
    verschiedenen Registerkarten

    ![A screenshot of a computer Description automatically
generated](./media-DE/image57.png)

3.  Wählen Sie unter "**Settings"** die Option **"Databases"**. Es
    sollten 3 databases aufgelistet sein.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image58.png)

4.  Klicken Sie auf **Migration** und dann auf die Schaltfläche **+
    Create**.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image59.png)

5.  Geben Sie auf der Seite **Migrate PostgreSQL to Azure Database for
    PostgreSQL Flexible Server** auf der Registerkarte **Setup** die
    folgenden Informationen an und klicken Sie dann auf **Next: Select
    Runtime Server**

    - Migration name - `PostgreSQLToAzurePG`

    - Source server – **On-premise Server**

    - Migration option – **Validate and Migrate**

    - Migration mode – **Online**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image60.png)

6.  Klicken Sie auf der Registerkarte **Select Runtime Server** auf
    **Next: Connect to source**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image61.png)

7.  Geben Sie auf der **Connect to source tab** die folgenden Details
    ein und klicken Sie auf **Next : Select migration target**

    - Server name – **Public IP address / DNS name of PostgresSrv VM**

    - Port – `5432`

    - Server admin login name - `postgres`

    - Password – `postgres`

    - SSL mode – **Prefer**

    - Test Connection – Klicken Sie auf **Connect to source**

    > **Warten Sie, bis die Testverbindung erfolgreich ist.**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image62.png)

8.  Geben Sie auf der Registerkarte **Select migration target** die
    folgenden Details an

    - Password - `P@55w.rd1234`

    - Test Connection - Klicken Sie auf **Connect to source**

    > **Warten Sie, bis die Testverbindung erfolgreich ist.**

    - Klicken Sie auf **Next : Select database(s) for migration**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image63.png)

9.  Wählen Sie auf der Registerkarte **Select database(s) for
    migration**, die Database **dvdrental** und klicken Sie dann auf
    **Next: Summary**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image64.png)

10. Überprüfen Sie auf der Registerkarte **Summary** die angezeigten
    Informationen und klicken Sie auf die Schaltfläche **Start
    Validation and Migration**.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image65.png)

11. Auf der Seite Migration, klicken Sie auf den Link
    the **PostgreSQLToAzurePG** 

    ![A screenshot of a computer Description automatically
generated](./media-DE/image66.png)

12. Klicken Sie auf der Seite **PostgreSQLToAzurePG** auf die
    Schaltfläche refresh, um die Aktualisierungen zu sehen.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image67.png)

13. Klicken Sie auf den Database name **dvdrental**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image68.png)

14. Auf der Registerkarte **Validation** sollten Sie die Details der
    Validierungsaufgaben sehen können.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image69.png)

15. Auf der Registerkarte **Migration** wird der Migrationsstatus in der
    Warteschlange angezeigt.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image70.png)

16. Klicken Sie auf der Seite **PostgreSQLToAzurePG** erneut auf die
    Schaltfläche refresh und stellen Sie fest, dass die
    Migrationsaufgaben ebenfalls abgeschlossen sind und nun die
    Schaltfläche **Warten auf Benutzeraktion** erscheint, klicken Sie
    auf die Schaltfläche **Cutover**.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image71.png)

17. Wenn Sie aufgefordert werden, **Please perform the following steps
    manually before doing the cutover**, klicken Sie auf die
    Schaltfläche **Yes**
    
    ![](./media-DE/image72.png)

    ![A white background with black text Description automatically
generated](./media-DE/image73.png)

18. Klicken Sie auf der Seite **PostgreSQLToAzurePG** erneut auf die
    Schaltfläche refresh. Der Status "Cutover in progress" sollte unter
    **Migration details** erscheinen.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image74.png)

19. Sobald der Cutover **abgeschlossen** ist, schließen Sie das
    **PostgreSQLToAzurePG** -Blade.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image75.png)

20. Zurück auf der **Migration** Seite können wir sehen, dass die
    Migration der PostgreSQL-Datenbank **erfolgreich war**.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image76.png)

21. Wählen Sie unter Settings **Databases**, wählen Sie **dvdrental**
    und klicken Sie auf die Schaltfläche **Connect**

    ![A screenshot of a computer Description automatically
generated](./media-DE/image77.png)

22. Wenn sich die Cloud Shell öffnet, wird sie nach dem Passwort fragen.
    Geben Sie das Passwort als `P@55w.rd1234` ein.

    ![A screenshot of a computer Description automatically
generated](./media-DE/image78.png)

23. Sobald die Verbindung zur database erfolgreich hergestellt wurde,
    wird sie als **devrental=\** aufgeführt**.**

24. Führen Sie den folgenden Befehl aus, um die Tabellen in der
    Zieldatenbank aufzulisten

    `\dt`

    ![A screenshot of a computer Description automatically
generated](./media-DE/image79.png)

    > **Hinweis**: Diese Tabellen sind die gleichen wie in der Source
    database.

    ![A diagram of a computer Description automatically
generated](./media-DE/image36.png)

    > **Somit haben wir die On-premises PostgreSQL database to Azure Database
    for PostgreSQL Flexible Server** **migriert**.

## Zusammenfassung

Im diesem Lab haben wir eine virtuelle Maschine bereitgestellt, um die
PostgreSQL-database zu hosten, und dann haben wir die
PostgreSQL-database mit dem **Azure Database for Postgres Flexible
Server (Migration)** migriert.

![A screenshot of a computer Description automatically
generated](./media-DE/image47.png)

![A screenshot of a computer Description automatically
generated](./media-DE/image75.png)
