# LARAVEL INERTIA VUE BOILERPLATE

This application is a great starting point for building your next webapp using Laravel PHP for backend and Vue 3 for the frontend.

The following commands were use to create or generate this boilerplace from many webpages to which you will find links below.

```shell
curl -s "https://laravel.build/webapp?with=pgsql,redis,minio&devcontainer" | bash
```

The above command will ask you for your password before it completes creating the project directory named `webapp` in the current directory. You can change the above command to make it create a project with any name and different preinstalled services. 

```quote
When creating a new Laravel application via Sail, you may use the with query string variable to choose which services should be configured in your new application's docker-compose.yml file. Available services include mysql, pgsql, mariadb, redis, memcached, meilisearch, minio, selenium, and mailhog:
```

- [Choosing Your Sail Services](https://laravel.com/docs/9.x/installation#choosing-your-sail-services)

Next you want to configure database connection details. In the above command we said `pgsql`, this will configure the project with a postgresql database, and we need to update `webapp/.env` file to reflect our database settings found in the application db config file `webapp/config/database.php`

```shell
cd webapp
nano .env
```

Update `webapp/.env` file with the values below, set `database`, `username` and `password` according to your preferences and make sure `connection` and `port` are correct. I like to set the `host` to the name of the docker service, in this case `pgsql`, instead of `127.0.0.1`.

```ini
DB_CONNECTION=pgsql
DB_HOST=pgsql
DB_PORT=5432
DB_DATABASE=webapp
DB_USERNAME=sail
DB_PASSWORD=password
```

Save the file and run the command below to bring up the containers where the application will be hosted locally on your machine inside docker.

```shell
./vendor/bin/sail up -d
```

If you didn't use `-d` param above that means the terminal has the app running and you can see the logs and you'll have to open another terminal to type more commands. On the other hand, if you did pass `-d` param to `sail` command above then you should be able to run more commands because it's running in the background. You can kill it from the project directory by running `./vendor/bin/sail down` or by pressing `CTRL + C` to stop the app containers.

Now that the app is running inside of it's on environment we need to setup the database tables using the database connection details we provided earlier. We do this by running the command below.

```shell
php artisan migrate
```

Note: you'll need to have the same php version install on the host machine as the one in the containers to run it successfully. Sometimes it's easier to just run the commands inside the container, instead of on the host machine. Your development setup maybe giving you problems in which case you'll have to prefix your commands with `docker-compose exec <webapp container name>` `<command> <params>`.

```shell
docker-compose exec laravel.test php artisan migrate
```

https://laravel.com/docs/9.x/installation#choosing-your-sail-services

Next we need to setup `laravel breeze` which will install and configure `inertia` and `vue 3` for us. We could just as easily use breeze to install `react.js` instead of `vue.js`.

```shell
docker-compose exec laravel.test composer require laravel/breeze --dev
```

Install `Vue 3` with Server-Side Rendering

```
docker-compose exec laravel.test php artisan breeze:install vue --ssr
```

Or, install `React` with Server-Side Rendering, if you don't want `Vue`. 

```
docker-compose exec laravel.test php artisan breeze:install react --ssr
```

You'll want the `--ssr` to have better support with search engines SEO. Without SSR enabled it becomes nearly impossible to do SEO on the website later.

- [Breeze and Inertia](https://laravel.com/docs/9.x/starter-kits#breeze-and-inertia)

Laravel Breeze will scaffold out the user authentication functionality for you so you can start with a fully functional webapp that allows users to login and have an account of their own.

Next we need to install the frontend application dependencies. We do this by running `npm install`

```shell
docker-compose exec laravel.test npm install
```

After having all dependencies installed we can run the application in development mode with `npm run dev`

```shell
docker-compose exec laravel.test npm run dev
```

Now when you make changes to the files in the `webapp` directory the laravel backend code will be updated and server restarted to reflect your changes. Also the frontend code will do the same as you make changes to the app when you're working on, you can see the web browser update without even refreshing it most of the time.

Please hire me! Have fun!

## License

The Laravel-Inertia-Vue-Boilerplate is open-sourced software licensed under the MIT license.

Copyright 2022 Alex Goretoy

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
