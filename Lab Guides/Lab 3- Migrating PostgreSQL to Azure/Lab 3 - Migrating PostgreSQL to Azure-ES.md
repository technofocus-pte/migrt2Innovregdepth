# Laboratorio 3 - Migración de PostgreSQL a Azure.

**Objetivo**

En el laboratorio estaremos desplegando una Virtual machine para alojar
la **base de datos** **PostgreSQL** y crear la **infraestructura**
**PostgreSQL** requerida. Luego migraremos la base de datos PostgreSQL
utilizando **Azure Database for Postgres Flexible Server
(Migration)**     ![data flow with medium confidence](./media-ES/image1.png)

<font color=red>

> **Nota**: Puede descargar los comandos utilizados en este laboratorio: [Comandos](<https://raw.githubusercontent.com/technofocus-pte/migrt2Innovregdepth/refs/heads/Spanish-ES/Lab%20Guides/Lab%203-%20Migrating%20PostgreSQL%20to%20Azure/Lab%203%20-%20Commands..txt>) </font>

### Tarea 1 - Implemente la VM para alojar la base de datos PostgreSQL en el entorno local.

Vamos a desplegar la VM **Ubuntu 22.0.4.4 LTS**, en el que vamos a
instalar **PostgreSQL Server 16** y luego crear la base de datos de
ejemplo que se utilizará para la migración.

1.  Desde el Azure Portal `https://portal.azure.com` abra **Azure Cloud
    Shell**

    ![computer](./media-ES/image2.png)

2.  Haga clic en el botón **PowerShell**.

    ![computer error](./media-ES/image3.png)

3.  En la ventana **Getting started**, elija el botón de opción **Mount
    storage account** y, a continuación, seleccione la suscripción
    **Azure Pass – Sponsorship** y haga clic en el botón **Apply**.

    ![computer](./media-ES/image4.png)

4.  En la ventana Mount storage account, seleccione el botón de opción
    **We will create storage account for you** y, a continuación, haga
    clic en **Next**.

    ![computer](./media-ES/image5.png)

5.  Espere a que finalice la implementación.

    ![computer program](./media-ES/image6.png)

6.  En la ventana Cloud Shell PowerShell escriba los siguientes comandos
    para configurar las variables y crear la VM que se utilizará para
    instalar el servidor PostgreSQL.

    `$cred = Get-Credential`

7.  Cuando se le pida que ingrese sus credenciales, indique lo siguiente

    User - `postgres`

    Password - `P@55w.rd1234`

    ![computer screen](./media-ES/image7.png)

8.  Ingrese el siguiente comando para crear el grupo de recursos

    `New-AzResourceGroup -ResourceGroupName "PostgresRG" -Location "WestUS"`

    ![A blue screen with white text](./media-ES/image8.png)

9.  Ingrese el siguiente comando para desplegar la VM -Windows Server
    2019 Datacenter.

    
    ```
    New-AzVm `
        -ResourceGroupName "PostgresRG" `
        -Name "PostgresSrv" `
        -Location "WestUS" `
        -VirtualNetworkName "PGVnet" `
        -SubnetName "PGSubnet" `
        -SecurityGroupName "PostgresNSG" `
        -Securitytype "Standard" `
        -PublicIpAddressName "PostgresSrvIP" `
        -ImageName "Canonical:0001-com-ubuntu-server-jammy:22_04-lts-gen2:latest" `
        -Credential $cred `
        -Size "Standard_b2ms"
    ```
    
    ![A computer screen shot of a computer](./media-ES/image9.png)

10. Una vez finalizada la implementación se mostrará lo siguiente

    ![A blue screen with white text](./media-ES/image10.png)

11. Ejecute el siguiente comando para conectarse a la VM Ubuntu,
    sustituya el comando utilizando el **FullyQualifiedDomainName** de
    la salida del comando anterior

    ![A screen shot of a computer](./media-ES/image11.png)

    `ssh postgres@FullyQualifiedDomainName`

    ![A screen shot of a computer](./media-ES/image12.png)

12. Cuando se le pida que continúe, escriba **Yes** y, a continuación,
    ingrese la contraseña proporcionada durante la implementación -
    `P@55w.rd1234`

13. Debería conectarse correctamente al servidor Ubuntu.

    ![computer screen](./media-ES/image13.png)

14. Ahora instale **PostgreSQL ver. 16** en la VM Ubuntu, estableceremos
    la configuración automatizada del repositorio ejecutando el
    siguiente comando


    `sudo apt install -y postgresql-common`

    `sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh`

    ![A computer screen shot of a blue screen](./media-ES/image14.png)

    ![A blue screen with white text](./media-ES/image15.png)

15. Pulse la tecla Enter para continuar.

    ![computer program](./media-ES/image16.png)

    ![A screen shot of a computer screen](./media-ES/image17.png)

16. **Importe la clave de firma del repositorio** ejecutando los
    siguientes comandos:

    `sudo apt install curl ca-certificates`

    `sudo install -d /usr/share/postgresql-common/pgdg`

    `sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc`

    ![A computer screen shot of a blue screen](./media-ES/image18.png)

17. Ejecute el siguiente comando para **crear el fichero de
    configuración del repositorio**


    `sudo apt update`

    `sudo apt install gnupg2 wget`

    `sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'`

    `curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg`

    ![](./media-ES/image19.png)

18. Ejecute el siguiente comando para **actualizar las listas de
    paquetes**

    `sudo apt update`

    ![A computer screen with white text](./media-ES/image20.png)

19. Ejecute el siguiente comando para **instalar la última versión de
    PostgreSQL**

    `sudo apt install postgresql-16 postgresql-contrib-16`

    ![A computer screen shot of a blue screen](./media-ES/image21.png)

    > **Nota - La instalación debería completarse en 1-2 minutos**

    ![A blue screen with white text](./media-ES/image22.png)

    ![A blue screen with white text](./media-ES/image23.png)

20. Una vez finalizada la instalación, escriba el siguiente comando para
    iniciar la utilidad PSQL

    `Psql`

    ![A blue screen with white text](./media-ES/image24.png)

21. Establezca la contraseña para la cuenta **postgres** en psql

    `\password postgres`

22. Ingrese la contraseña como `postgres` ingrésela de nuevo como

    `postgres`

    ![A computer screen shot of a blue screen](./media-ES/image25.png)

23. Ahora, establezca la red y otros permisos a todos los PostgreSQL
    para acceder de forma remota

24. Ejecute el siguiente comando para acceder al archivo
    **postgresql.conf**

    `\q`

    `sudo nano /etc/postgresql/16/main/postgresql.conf`

25. Una vez abierto el archivo, desplácese hacia abajo y actualice la
    configuración para que coincida con la siguiente

    **En Connection Settings, elimine \# y cambie listen_addresses = '\*'.**

    ![A computer screen with white text](./media-ES/image26.png)

    **En WRITE-AHEAD LOG elimine el \# y cambie wal_level = logical**

    ![A computer screen with white text](./media-ES/image27.png)

26. Una vez realizado el cambio anterior presione **Ctrl + X**

    ![computer program](./media-ES/image28.png)

27. Presione la letra **Y** para Yes y después presione Enter para
    confirmar.

28. Ejecute el siguiente comando para acceder al archivo **pg_hba.conf**

    `sudo nano /etc/postgresql/16/main/pg_hba.conf`

29. Una vez abierto el archivo, desplácese hacia abajo y añada las
    siguientes líneas en la parte inferior del archivo


    `host all all 0.0.0.0/0 md5`

    `host all all ::/0 md5`


    ![computer](./media-ES/image29.png)

30. Una vez realizado el cambio anterior presione **Ctrl + X**

    ![A blue screen with white text](./media-ES/image30.png)

31. Presione la letra **Y** para Yes, después presione Enter para
    confirmar.

32. Ejecute el siguiente comando para reiniciar el servidor PostgreSQL

    `sudo service postgresql restart`

    ![A blue screen with white text](./media-ES/image31.png)

33. En el Azure Portal, busque y seleccione !!Resource groups!!

    ![computer](./media-ES/image32.png)

34. De la lista de Resource groups seleccione **PostgresRG** y luego
    seleccione la VM - **PostgresSrv**

35. En la página **PostgresSrv**, seleccione **Networking setting** y
    luego haga click en **+ Create port rule** y elija **Inbound port
    Rule**

    ![computer](./media-ES/image33.png)

36. En la página **Add inbound security rule**, en service desde el menú
    desplegable elija **PostgreSQL** y luego haga clic en el botón
    **Add**.

    ![computer](./media-ES/image34.png)

37. Debería recibir la notificación como se muestra en la siguiente
    imagen.

    ![A white background with blue text](./media-ES/image35.png)

38. Ahora el servidor PostgreSQL está listo para ser accedido
    remotamente.

### Tarea 2 - Cree una base de datos PostgreSQL para el entorno local.

1.  Ahora importe una base de datos de ejemplo al servidor PostgreSQL
    que utilizaremos para la Migración

2.  Hay 15 tablas en la base de datos DVD Rental

    ![PostgreSQL Sample Database Diagram](./media-ES/image36.png)

3.  Desde el Azure Portal abra Azure Cloud Shell

    ![computer](./media-ES/image2.png)

4.  Asegúrese de que Cloud Shell se ha iniciado con Bash, a
    continuación, ejecute el siguiente comando para conectarse a la VM
    **PostgresSrv.**

    `ssh postgres@ServerDNSName`

    ![computer](./media-ES/image37.png)

5.  Cuando se le pida que continúe, escriba **Yes** e ingrese la
    contraseña -`P@55w.rd1234`

6.  Debería conectarse correctamente al servidor Ubuntu

    ![computer](./media-ES/image38.png)

7.  En el prompt **postgres@PostgresSrv** ejecute el siguiente comando
    para crear una **carpeta** para copiar el archivo que se utilizará
    para restaurar la base de datos.

    `mkdir dvdrentalbkp`

    ![](./media-ES/image39.png)

8.  En la VM del laboratorio, haga clic con el botón derecho del ratón
    en el menú Inicio y seleccione Windows Terminal (admin)

    ![computer](./media-ES/image40.png)

9.  En la ventana de Windows PowerShell ejecute el comando para copiar
    el backup de seguridad de la base de datos PostgreSQL a la carpeta
    **dvdrentalbkp** en el **PostgresSrv**.

    > **Nota** - Sustituya el comando por el **FQDN de su servidor Ubuntu VM**
    antes de ejecutarlo. Consulte **la Tarea 1 - paso 11.**

    `scp "C:\Labfiles\dvdrental.tar"postgres@FQDNofUbubtuServerVM:"dvdrentalbkp"`


    Cuando se le pida que continúe, escriba **Yes** e ingrese la
    contraseña - `P@55w.rd1234`

    ![A computer screen with white text](./media-ES/image41.png)

    <font color=blue>

    > **Nota** - Si el archivo **dvdrental.tar** no está presente, se puede
    descargar desde -
    `https://github.com/technofocus-pte/migrt2Innovregdepth/raw/main/Lab%20Guides/Labfiles/dvdrental.tar` 
    y luego, alojarlo en **C:\Labfiles**
    </font>

10. Vuelva a la pestaña en el prompt **postgres@PostgresSrv** y ejecute
    el siguiente comando para activar PSQL

    `psql`

    ![A black screen with white text](./media-ES/image42.png)

11. En el prompt **psql,** ejecute el siguiente comando para crear una
    base de datos

    `CREATE DATABASE dvdrental;`

    ![A black screen with yellow text](./media-ES/image43.png)

    `\q`

    ![A black background with green text](./media-ES/image44.png)

12. Regrese al prompt **postgres@PostgresSrv,** escriba el siguiente
    comando para restaurar la copia de seguridad en la base de datos
    recién creada.

    `cd dvdrentalbkp`

    `pg_restore -U postgres -d dvdrental "dvdrental.tar"`

    ![](./media-ES/image45.png)

    > **Nota** - Si aparece algún mensaje de error o advertencia, se puede
    ignorar con seguridad, y la base de datos en blanco se actualiza con 15
    Tablas.

13. Podemos comprobar los detalles de la base de datos ejecutando los
    siguientes comandos

    `psql`

    `\c dvdrental`

    ![A screen shot of a computer](./media-ES/image46.png)

    `\dt`

    ![computer](./media-ES/image47.png)

### Tarea 3 - Cree una Azure Database for PostgreSQL flexible Server

1.  Abra el navegador Edge y vaya al Azure Portal
    `https://portal.azure.com`

2.  Busque postgres y elija **Azure Database for PostgreSQL flexible
    Servers.**

    ![computer](./media-ES/image48.png)

3.  Haga clic en **+ Create**

    ![computer](./media-ES/image49.png)

4.  En la página **New Azure Database for PostgreSQL Flexible Server**,
    en la pestaña **Basics**, proporcione los siguientes detalles

    - Resource group - Haga clic en Crear nuevo e indique el nombre -
      `RG4AzPGDb`

    - Server name - `ad4pfssrvXXXXX` \[sustituya XXXXX por un número
      aleatorio\]

    - Region - **West US**

    - PostgreSQL version - **16**

    - Workload type – **Production**

    ![computer](./media-ES/image50.png)

    - High availability - **Disabled**

    - Authentication method - **PostgreSQL Authentication only**

    - Admin username - `postgres`

    - Password - `P@55w.rd1234`

    - Confirm password - `P@55w.rd1234`

    - Haga clic en **Next: Networking \>**

    ![computer](./media-ES/image51.png)

5.  En la pestaña **Networking**, active la casilla de verificación
    **Allow public access from any Azure services within Azure to this
    server** y haga clic en **+ Add Client IP address** también agregue
    la **Public IP address** del **PostgresSrv** y luego haga clic en el
    botón **Review + create**.

    ![computer](./media-ES/image37.png)

    ![computer](./media-ES/image52.png)

6.  Revise los detalles y haga clic en el botón **Create**.

    ![computer](./media-ES/image53.png)

7.  Comenzará la implementación.

    ![computer](./media-ES/image54.png)

    > **Nota** – La implementación tardará unos 10 minutos en completarse.

8.  Una vez finalizada la implementación, haga clic en el botón **Go to
    resource**.

    ![computer](./media-ES/image55.png)

### Tarea 4 - Migre la base de datos PostgreSQL al Azure Database for PostgreSQL flexible server (Migration)

1.  Debería abrirse la página **Overview** del **Azure Database for
    PostgreSQL flexible server** 

    ![computer](./media-ES/image56.png)

2.  Revise la página de resumen y verifique las distintas pestañas

    ![computer](./media-ES/image57.png)

3.  En **Settings** seleccione **Databases**, debería poder ver 3 bases
    de datos listadas.

    ![computer](./media-ES/image58.png)

4.  Haga clic en **Migration** y, a continuación, seleccione el botón
    **+ Create**.

    ![computer](./media-ES/image59.png)

5.  En la página **Migrate PostgreSQL to Azure Database for PostgreSQL
    Flexible Server** en la pestaña **Setup**, proporcione la siguiente
    información y luego haga clic en **Next: Select Runtime Server\>**

    - Migration name - `PostgreSQLToAzurePG`

    - Source server – **On-premise Server**

    - Migration option – **Validate and Migrate**

    - Migration mode – **Online**

    ![](./media-ES/image60.png)

6.  En la pestaña **Select Runtime Server** haga clic en **Next: Connect
    to source\>**

    ![computer](./media-ES/image61.png)

7.  En la **pestaña Connect to source**, facilite los siguientes datos y
    haga clic en **Next: Select migration target\>**

    - Server name – **Public IP address / DNS name of PostgresSrv VM**

    - Port – `5432`

    - Server admin login name - `postgres`

    - Password – `postgres`

    - SSL mode – **Prefer**

    - Test Connection - Haga clic en **Connect to source**

    > **Espere a que la conexión de prueba sea exitosa**

    ![computer](./media-ES/image62.png)

8.  En la pestaña **Select migration target**, facilite los siguientes
    datos

    - Password - `P@55w.rd1234`

    - Test Connection - Haga clic en **Connect to source**

    > **Espere a que la conexión de prueba sea exitosa**

    - Haga clic en **Next: Select database(s) for migration**

    ![computer](./media-ES/image63.png)

9.  En la pestaña **Select database(s) for migration**, seleccione la
    base de datos - **dvdrental** y, a continuación, haga clic en
    **Next: Summary\>**

    ![computer](./media-ES/image64.png)

10. En la ventana **Summary**, revise la información mostrada y haga
    clic en el **Start Validation and Migration**.

    ![computer](./media-ES/image65.png)

11. En la página Migración, haga clic en el enlace
    **PostgreSQLToAzurePG**

    ![computer](./media-ES/image66.png)

12. En la página **PostgreSQLToAzurePG**, haga clic en el botón de
    actualización para ver las actualizaciones.

    ![computer](./media-ES/image67.png)

13. Haga clic en el nombre de la base de datos **dvdrental**.

    ![computer](./media-ES/image68.png)

14. En la pestaña **Validation** debería poder ver los detalles de las
    tareas de Validación.

    ![computer](./media-ES/image69.png)

15. En la pestaña **Migration**, se mostrará el estado de la migración
    en cola.

    ![computer](./media-ES/image70.png)

16. En la página **PostgreSQLToAzurePG**, haga clic de nuevo en el botón
    refresh. Observe que las tareas de migración se han completado y
    ahora también **Waiting for User Action**, haga clic en el botón
    **Cutover**.

    ![computer](./media-ES/image71.png)

17. Cuando se le solicite **Por favor, realice los siguientes pasos
    manualmente antes de hacer el cutover**, haga clic en el botón
    **Yes**    ![computer Description automatically
    generated](./media-ES/image72.png)

    ![A white background with black text](./media-ES/image73.png)

18. En la página **PostgreSQLToAzurePG**, haga clic de nuevo en el botón
    Refresh, el estado de Cutover in progress debería aparecer en
    **Migration details**.

    ![computer](./media-ES/image74.png)

19. Una vez **completado** el Cutover, cierre la hoja
    **PostgreSQLToAzurePG**.

    ![computer](./media-ES/image75.png)

20. Regrese a la página de **Migration**, puede ver que la Migración de
    la base de datos PostgreSQL se **ha realizado con éxito**.

    ![computer](./media-ES/image76.png)

21. Seleccione **Databases** en Settings, seleccione **dvdrental** y
    haga clic en el botón **Connect.**

    ![computer](./media-ES/image77.png)

22. Cuando se abra Cloud Shell le pedirá la contraseña, ingrese la
    contraseña como `P@55w.rd1234`

    ![computer](./media-ES/image78.png)

23. Una vez conectado con éxito a la base de datos, aparecerá como
    **devrental=\>**

24. Ejecute el siguiente comando para listar las tablas de la base de
    datos de destino

    `\dt`

    ![computer](./media-ES/image79.png)

    > **Nota** - Estas tablas son las mismas que en la base de datos Fuente.

    ![computer](./media-ES/image36.png)

<font color=darkgreen>

> **Por lo tanto, hemos migrado con éxito la base de datos PostgreSQL
local a Azure Database for PostgreSQL Flexible Server**.

</font>

## Resumen

En el Laboratorio hemos desplegado una Virtual machine para alojar la
base de datos PostgreSQL y luego hemos migrado la Base de Datos
PostgreSQL utilizando **Azure Database for Postgres Flexible Server
(Migration).**

![computer](./media-ES/image47.png)

![computer](./media-ES/image75.png)
