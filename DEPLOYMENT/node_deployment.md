## BACKEND API SETUP
/var/www/html/arhetype-backend-api
cd /var/www/html/arhetype-backend-api
npm install
nano ecosystem.config.js

module.exports = {
  apps: [
    {
      name: "archetype-api",
      script: "index.js",
      cwd: "/var/www/html/arhetype-backend-api",
      instances: 1,
      exec_mode: "fork",
      env: {
        NODE_ENV: "production",
        PORT: 5000
      }
    }
  ]
};

sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod rewrite
sudo systemctl restart apache2

sudo nano /etc/apache2/sites-available/archetype-api.conf
<VirtualHost *:80>
    ServerName archetype-api.brande-staging.com

    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/

    ErrorLog ${APACHE_LOG_DIR}/archetype-api-error.log
    CustomLog ${APACHE_LOG_DIR}/archetype-api-access.log combined
</VirtualHost>
sudo a2ensite archetype-api.conf

pm2 start ecosystem.config.js
pm2 save
pm2 list

## FRONTEND SETUP
/var/www/html/Archetype-dashboard
cd /var/www/html/Archetype-dashboard
npm install
npm run build

/var/www/html/Archetype-dashboard/build


sudo nano /etc/apache2/sites-available/archetype-dashboard.conf

<VirtualHost *:80>
    ServerName archetype-dashboard.brande-staging.com
    DocumentRoot /var/www/html/Archetype-dashboard/build

    <Directory /var/www/html/Archetype-dashboard/build>
        Options -Indexes +FollowSymLinks
        AllowOverride None
        Require all granted

        # React Router support (SAFE)
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteRule . index.html [L]
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/archetype-dashboard-error.log
    CustomLog ${APACHE_LOG_DIR}/archetype-dashboard-access.log combined
</VirtualHost>

sudo a2enmod rewrite
sudo a2ensite archetype-dashboard.conf

sudo apachectl configtest
sudo systemctl reload apache2

pm2 startup
pm2 save

## Enable HTTPS
sudo certbot --apache