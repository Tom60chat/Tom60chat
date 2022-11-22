# 8 - PHP

1. `apt install php`
2. `nano /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai1.php`
   ```php
   <?php
    echo phpversion()."\n";
   ?>
   ```
   `curl w1.53.lprgi.u13.org/essai1.php`
3. `nano /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai2.php`
   ```php
   <?php
    echo "<html><body>Hello World : ".posix_getuid()."</body></html>";
   ?>
   ```
   `curl w1.53.lprgi.u13.org/essai2.php`
4. `nano /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai3.php`
   ```php
   <?php
    include("/etc/passwd");
   ?>
   ```
   `curl w1.53.lprgi.u13.org/essai3.php`
5. `nano /var/www/vhosts/w1.XX.lprgi.u13.org/html/essai4.php`
   ```php
   <?php
    echo "<html><body>";
    echo "Liste :<br /><pre>";
    $pointeur = opendir("/var/www");
    while ($fichier = readdir($pointeur))
    {
    echo "$fichier\n";
    }
    echo "</pre></body></html>";
   ?>
   ```
   `curl w1.53.lprgi.u13.org/essai4.php`
6. `find /etc/php* -name "php.ini" -print`