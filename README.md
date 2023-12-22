# aupost - additional settings
#2023-12-23
altered invoice to display pckup if no delivery address 
constants use \admin\includes\extra_datafiles\dist-site_specific_admin_overrides.php
so only available for zen cart 1.5.8+

# 2023-04-29
 extra changes to improve zc if posting in Australia
 
Address formats
===============
Zen cart v1.5.8 changed the address format for Australia to format number 7. By default the Country will be printed / displayed on address labels.
Address formats are stored in the address_format table.
Address format 1 is : $firstname $lastname$cr$streets$cr$city$cr $state $postcode
Address format 7 is : $firstname $lastname$cr$streets$cr$city$cr$state $postcode$cr$country
ZC documention on address formats in on the website at https://docs.zen-cart.com/user/localization/address_formats/
The preferred address format for posting within Australia is not the display the country name. This also removes an extra line on the invoice.

NOTE: Perform these actions on a test data base first and ALWAYS back your main data base before making and data changes.
To change the address format for the delivery address the following SQL statement can be run from admin > Tools > Install SQL Patches
Enter the following SQL statement in the query window and click Send

UPDATE orders SET delivery_address_format_id = 1 WHERE delivery_country = 'Australia';
The displayed address for Australian addresses will no longer display the country.

Invoices
========
Australia has a legal requirement to display the words "Tax Invoice" on an invoice.
I have modified the invoice to reflect this. The invoice file is  admin/invoice.php
Changes required are: 
    modify the invoice.php file to include an invoice title
    create a definition for the title in admin/languages/english/lang.invoice.php;and
    create a new css class for teh title
In invoice.php file at approx line 168 I have added  the following
        <!-- //BMH change to align right -->
        <br>
        <span class="pageHeadingTax"><?php echo TITLE_INVOICE;?> </span> 
        <!-- BMH eof -- -->
         
Additional changes to invoice.php can be seen in the included invoice.php file. These include:
    changing the formatting to improve foldabity for inclusion in address sleeves/ labels  
    widening comments fields
    checking the for IMAGE_ON_INVLICE_IMAGE_SHOW
    
    
Language definition files
-------------------------
The file admin/includes/languages/land.english.php has been modified to suite Australian dates and add additional definitions.
date formats should be @setlocale(LC_TIME, ['en_AU', 'en_AU.utf8mb4', 'en', 'English_Australia.1252']);
all displayed dates should be in the format dd/MM/yyyy
weight units should be in grams
In admin/languages/english/lang.invoice.php I have added an Invoice title and a check for IMAGE_ON_INVOICE_IMAGE_SHOW. See the included file
In the css file I have added:
pageHeadingTax
eg
/* ==  Invoice  ======================== */
tbody > tr > td.pageHeading {line-height:1.1em;} /* invoice banner address */
TD.pageHeadingTax {vertical-align: bottom !important; font-size:2em; font-weight:600;}
#shipto {width: 100%; float: right;}

All of my additional css settings are in the included css files. I have renamed the original css file "stylesheet.css" to "stylesheet_ori.css" and created a new "stylesheet.css" to call the following style sheets
    @import url("stylesheet_ori.css"); 
    @import url("bootstrap-zc-admin-overide.css"); 
    @import url("stylesheet_xbmh.css");  
    @import url("stylesheet_xbanner.css");  
    
    This includes the original css settings for admin + my overides for the bootstrap css file + my override css  settings (as they are loaded last they take precedence) + an additional stylesheet that changes the banners for a site.
    I change the background color of the login page and also the top headerBar background-color so can I alway tell at a glance which site I am logged into. Eg production is darkred, pre-production build is purple, development is green, test is brown and vanilla is a light yellow.



address_table
dataTableContentzz




