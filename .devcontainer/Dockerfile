# Base image for Codespace (Debian-based)
FROM mcr.microsoft.com/vscode/devcontainers/python:3.10

# Install Odoo dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    nodejs \
    npm \
    postgresql \
    libpq-dev \
    build-essential \
    python3-dev \
    python3-pip \
    libxml2-dev \
    libxslt1-dev \
    zlib1g-dev \
    libsasl2-dev \
    libldap2-dev \
    libjpeg-dev \
    libpng-dev \
    libffi-dev \
    libssl-dev

# Install wkhtmltopdf (required by Odoo)
RUN apt-get install -y xfonts-75dpi xfonts-base libxrender1 libxext6 libfontconfig1 libfreetype6 fontconfig-config fonts-dejavu-core && \
    wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.bionic_amd64.deb && \
    dpkg -i wkhtmltox_0.12.6-1.bionic_amd64.deb && \
    apt-get -f install -y

# Clone Odoo repository
RUN git clone https://github.com/sheeju/odoo /opt/odoo

# Install Odoo Python dependencies
WORKDIR /opt/odoo
RUN pip install -r requirements.txt

# Set up PostgreSQL
RUN service postgresql start && \
    sudo -u postgres createuser --createdb --username postgres odoo && \
    sudo -u postgres createdb --username postgres --owner=odoo odoo

# Expose Odoo service
EXPOSE 8069

# Start Odoo server
CMD ["./odoo-bin", "-d", "odoo", "--dev", "all"]
