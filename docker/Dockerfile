# Dockerfile
FROM php:7.4-fpm

LABEL Maintainer="Oliver Adria <oliver@adria.dev>" \
      Description="Base setup for web development with PHP and PostgreSQL."

# Get frequently used tools
RUN apt-get update && apt-get install -y \
    build-essential \
    libicu-dev \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libonig-dev \
    locales \
    zip \
    unzip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    wget \
    zsh

RUN docker-php-ext-install \
        bcmath \
        mbstring \
        pcntl \
        intl \
        zip


RUN apt-get clean && rm -rf /var/lib/apt/lists/*


# Install oh-my-zsh
RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true

# Copy powerlevel10k zsh theme
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

# Change the default theme, so to differentiate in case you use the default theme on your host
RUN sed -i 's:robbyrussell:powerlevel10k/powerlevel10k:g' ~/.zshrc

# Copy minimalistic powerlevel10k configuration, run "p10k configure" to go through wizard
COPY docker/.p10k.zsh /root/

# When running zsh, activate the p10k
RUN echo '[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh' >> ~/.zshrc

# Copy existing app directory
COPY . /var/www

WORKDIR /var/www

# Copy and run composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
RUN composer install --no-interaction

EXPOSE 8080

CMD ["php-fpm"]
