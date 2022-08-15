# motshoenengl-drupal9
Expose endpoint
Steps taken to setup this assesment:
note: I was using Windows 10.
1. I Downloaded the latest Lando Windows .exe installer from GitHub link: https://github.com/lando/lando/releases/download/v3.6.4/lando-x64-v3.6.4.exe.
2. Double-click on downloaded lando.exe and go through the setup workflow.
To Setup Lando with Drupal
1. I created a folder in C:\motshoenengl-drupal9\lando-demo
2. In this folder I RUN composer command: "composer create-project drupal/recommended-project lando-demo" this command will get drupal files for me
3. Initialising Lando:
Now in cmd, let’s switch to the Drupal root directory(C:\motshoenengl-drupal9\lando-demo) and follow these steps :
Step 1: ‘Lando init’ command - After running this command you will see the below option.
Step 2: Choose recipe - After choosing “Current working directory”, the recipe list will show up. Choose the Drupal 9 recipe.
Step 3:  Web root - After choosing the recipe, you will now see the web root option. Select “web” option. "Web" folder is generated during Drupal installation using composer.
Step 4: App name - After choosing the Web root folder, you will see an option for app name. For this assessment, I named it “nolo-lando”.

After successfully completing the above installation steps, a “.lando.yml” file will be generated with the above basic details.

Drupal installation:
After setting up Drupal and initializing Lando, you will now need a local development URL. So let us use the "Lando Start" command. After running this command I got this details and urls:
 NAME            nolo-lando
 LOCATION        C:\motshoenengl-drupal9\lando-demo
 SERVICES        appserver, database
 APPSERVER URLS  https://localhost:54483
                 http://localhost:54484
                 http://nolo-lando.lndo.site/
                 https://nolo-lando.lndo.site/
                 
 You will now need the database name, database password and host name. For that, I run the "Lando info" command. and it gave me this details:
 [ { service: 'appserver',
    urls:
     [ 'https://localhost:54483',
       'http://localhost:54484',
       'http://nolo-lando.lndo.site/',
       'https://nolo-lando.lndo.site/' ],
    type: 'php',
    healthy: true,
    via: 'apache',
    webroot: 'web',
    config: { php: 'C:\\Users\\motshoenengl\\.lando\\config\\drupal9\\php.ini' },
    version: '8.0',
    meUser: 'www-data',
    hasCerts: true,
    hostnames: [ 'appserver.nololando.internal' ] },
  { service: 'database',
    urls: [],
    type: 'mysql',
    healthy: true,
    internal_connection: { host: 'database', port: '3306' },
    external_connection: { host: '127.0.0.1', port: '54491' },
    healthcheck: 'bash -c "[ -f /bitnami/mysql/.mysql_initialized ]"',
    creds: { database: 'drupal9', password: 'drupal9', user: 'drupal9' },
    config: { database: 'C:\\Users\\motshoenengl\\.lando\\config\\drupal9\\mysql.cnf' },
    version: '5.7',
    meUser: 'www-data',
    hasCerts: false,
    hostnames: [ 'database.nololando.internal' ] } ]
    
 Now you copy one url (http://nolo-lando.lndo.site/) and paste on the browser, then follow Drupal setup and use this internal_connection to setup Drupal 9
 
 To clone this Repo use: https://github.com/HloniDev/motshoenengl-drupal9.git
 
 Once Drupal is Installed and setup:
 To espose JSON:API endpoint:
 Goto structure-content type
 Add content type: mobile_phone_product with fields, sku, price, name and decription 
 I created 1 content to see if I will get it when requesting from postman
 to expose endpoint I enabled JSON:API modules this includes serialisation and REST API
 then Configuration -> REST -> then click on edit of the content row then only check "GET" method
 then the endpoint to get this mobile_phone_product contents is: http://localhost:54484/jsonapi/node/mobile_phone_product, I've added screenshots in motshoenengl-  drupal9\project working screenshots\endpoint GET request on postman.jpg
 
 I added this in my settings.php under drupal sites folder 
 $settings['trusted_host_patterns'] = [
  '^'.getenv('LANDO_APP_NAME').'\.lndo\.site$',      # lando proxy access
  '^localhost$',                                     # localhost access
  '^'.getenv('LANDO_APP_NAME').'\.localtunnel\.me$', # lando share access
  '^192\.168\.1\.100$'                               # LAN IP access
];

 

