# Moodle com Podman (Ambiente Local)

Este repositório contém a configuração completa para executar o **Moodle 5.x** localmente utilizando **Podman** e **podman-compose**, com banco de dados **MariaDB** e persistência de dados.

O ambiente foi estruturado para ser:
- Reprodutível
- Didático
- Compatível com boas práticas modernas do Moodle (uso de `/public`, Composer, UTF-8, etc.)

---

## 📁 Estrutura de diretórios

```text
.
├── mariadb
│   └── conf
│       └── moodle.cnf          # Configuração de charset/collation do MariaDB
├── moodle
│   ├── config
│   │   ├── 000-default.conf    # VirtualHost do Apache (DocumentRoot = /public)
│   │   └── config.php          # Configuração principal do Moodle
│   └── Dockerfile              # Imagem do Moodle (PHP + Apache)
├── podman-compose.yml          # Orquestração dos containers
└── volumes
    ├── db                      # Dados persistentes do MariaDB
    └── moodledata              # Diretório de dados do Moodle

```

## 🧰 Pré-requisitos

Antes de iniciar, certifique-se de ter instalado no host:

- Podman ≥ 4.x
- podman-compose
- Acesso à internet (para baixar imagens e dependências)

Verificação rápida:

```sh
podman --version
podman-compose --version
```

## ⚙️ Configuração do ambiente

### 1. Criar diretórios de volume (se não existirem)

```sh
mkdir -p volumes/db volumes/moodledata
chmod 0777 volumes/moodledata
```

> O diretório moodledata precisa ser gravável pelo usuário do Apache dentro do container.

### 2. Configuração do MariaDB (UTF-8)

O arquivo mariadb/conf/moodle.cnf já força:
- utf8mb4
- utf8mb4_unicode_ci

Isso é obrigatório para o Moodle.

⚠️ Importante:
Se você já tentou subir o ambiente antes, apague o volume do banco:

```sh
rm -rf volumes/db
```

### 3. Build e execução dos containers

Execute a partir da raiz do projeto:

```sh
podman-compose down
podman-compose build --no-cache
podman-compose up
```

> O `--no-cache` é importante para garantir que Composer e configurações sejam aplicadas corretamente.

## 🌐 Acesso ao Moodle

Após a inicialização, acesse no navegador:

```text
http://localhost:8080
```

Você verá o instalador do Moodle.

Siga os passos normalmente pela interface web.
