# Koristimo FrankenPHP dev image za PHP 8.5 (dok ne postane stabilan)
FROM dunglas/frankenphp:php8.5

# Postavljamo radni direktorij unutar kontejnera
WORKDIR /app

# Kopiramo naš kod
COPY . /app

# FrankenPHP po defaultu traži index.php u /app/public/
# Zato smo kreirali 'public' mapu lokalno.
