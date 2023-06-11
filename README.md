### Lando Laravel

- [Install Lando](https://docs.lando.dev/)
- [Trust CA](https://docs.lando.dev/config/security.html#trusting-the-ca)
- Run `lando start`
- Run `lando artisan db:seed`
- Run `lando artisan storage:link`
- Run `lando artisan user:create`
- Run `lando watch`
- 
- (Optional) when changing .lando.yml `lando rebuild -y`

### Access local enviroment
- [App](https://laravel.lndo.site)
- [Mailhog](http://mail.lndo.site)
- [Phpmyadmin](http://phpmyadmin.lndo.site)


### Lando Commands
- `lando start`
- `lando stop`
- `lando mysql`
- `lando ssh -s appserver`
- `lando logs -s appserver`
- [Lando Commands list](https://docs.lando.dev/cli/)
