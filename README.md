# LearnLaravel

## Required programs

- Git
- PHP 8.3 or newer and Composer
- Bun
- Docker with Docker Compose
- A code editor, such as VS Code (optional)

Laravel is installed by Composer as a project dependency.

### Windows (PowerShell)

```powershell
winget install --id Git.Git -e
winget install --id PHP.PHP.8.4 -e
winget install --id Composer.Composer -e
winget install --id Oven-sh.Bun -e
winget install --id Docker.DockerDesktop -e
winget install --id Microsoft.VisualStudioCode -e
```

Complete the installers, start Docker Desktop, and open a new terminal so PATH changes take effect. Docker Desktop may require WSL 2 and virtualization; follow its installer instructions.

### Arch Linux

```bash
sudo pacman -Syu git php composer bun docker docker-compose code
sudo systemctl enable --now docker
```

### Ubuntu

On Ubuntu 24.04 or newer, install the distribution packages:

```bash
sudo apt update
sudo apt install git php-cli php-mysql php-mbstring php-xml php-curl php-zip composer curl unzip docker.io docker-compose-v2
sudo systemctl enable --now docker
```

Install Bun using its official installer, then open a new terminal:

```bash
curl -fsSL https://bun.sh/install | bash
```

## Configure PHP extensions

Run `php --ini` to find the CLI configuration file and `php -m` to list loaded extensions. If no configuration file is loaded on Windows, copy `php.ini-development` to `php.ini` in the PHP installation directory. Edit the loaded file and uncomment the required extensions by removing the leading `;`, including:

```ini
extension=curl
extension=fileinfo
extension=mbstring
extension=openssl
extension=pdo_mysql
extension=zip
```

Set `extension_dir` to the PHP installation's `ext` directory if needed.

## Setup

Clone the repository and enter its directory:

```bash
git clone https://github.com/Ants-A/LearnLaravel.git
cd LearnLaravel
```

Copy the example configuration and install dependencies:

```bash
cp .env.example .env
composer install
bun install --frozen-lockfile
php artisan key:generate
```

In PowerShell, use `Copy-Item .env.example .env` instead of `cp` if preferred. Keep the generated `APP_KEY` private. The example configuration connects to the MariaDB container on port 33062 with the local development credentials in `docker-compose.yml`.

Start the database and run migrations:

```bash
docker compose up -d
php artisan migrate
```

If migrations fail because the database is still starting, wait a few seconds and retry. If port 33062 is already in use, change the published port in `docker-compose.yml` and `DB_PORT` in `.env` to match.

## Run the application

```bash
composer run dev
```

Open http://localhost:8000. Keep this terminal running while developing. To stop the database when finished, run `docker compose down` in another terminal; its named volume preserves the data.

To build frontend assets without the development server, run `bun run build`.
