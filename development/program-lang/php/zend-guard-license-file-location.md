License File Location
=====================

The location of the license file must be written into the php.ini file. This enables it to be validated and loaded into the license registry if valid. Depending on your setup you may need to restart the web server. For example: SAPI, ISAPI configurations that run a persistent PHP module require restarting the Web server to apply changes to the PHP engine.

Licenses are either valid or not-valid. The state "No license found" is treated as an invalid license. Therefore, proper license installation is necessary.

You can define specific files or license file directories to contain the license. If you specify a license directory, all files with the file extension zl, are checked for validity. If valid, they are loaded into the license registry, once, at the startup of the PHP server.

Files encoded to check for valid licenses will check the license registry for a license matching its product specification.

After updating the php.ini restart the server to apply changes for SAPI/ISAPI configurations.

The directive in the php.ini file for license paths is zend_loader.license_path.

The syntax is as follows:

    zend_loader.license_path=LicensePath1:LicensePath2

Where: LicensePath is the path to the file or directory holding the correct license file. For UNIX, multiple paths are entered separated by colons (colon delimited.)  For Windows, multiple paths are entered separated by semicolon (semicolon delimited.) 
 
### Examples:

The following lines specify two license files (UNIX).

    zend_loader.license_path=/usr/local/Zend/licenses/Lic.zl:/usr/local/Zend/licenses/Lic2.zl
 
The following line specifies one license file and a license folder (Windows).

    zend_loader.license_path=C:\dir1;C:\dir2;C:\dir3\lic.zl
