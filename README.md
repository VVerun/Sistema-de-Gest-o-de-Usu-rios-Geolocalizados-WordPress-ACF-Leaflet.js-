# Sistema-de-Gest-o-de-Usu-rios-Geolocalizados-WordPress-ACF-Leaflet.js-
Sistema web administrativo desenvolvido em WordPress com integração REST API e Leaflet.js para gerenciamento de usuários geolocalizados em mapa interativo, com controle de ranks, filtros avançados e edição dinâmica de localização.

# Click & Cria Maps

# Click & Cria Maps

Sistema administrativo de gerenciamento de usuários geolocalizados com integração ao WordPress REST API, ACF e Leaflet.js.

O sistema permite cadastro, classificação por rank, visualização em mapa interativo e edição dinâmica de localização e dados.

---

## Tecnologias Utilizadas

- WordPress (Custom Post Type)
- REST API WP
- Advanced Custom Fields (ACF)
- Leaflet.js
- OpenStreetMap (Nominatim)
- JavaScript (Vanilla)
- HTML5 e CSS3
- AJAX / Fetch API

---

## Funcionalidades

### Cadastro Administrativo
- Criação de usuários com:
  - Nome
  - Telefone
  - Setor
  - Status
  - Rank (taxonomia)
  - Endereço digitado
  - Latitude e Longitude
  - Upload de foto
- Integração completa com REST API
- Upload de imagem via `/wp/v2/media`

### Cadastro Público
- Rank fixo como "Em análise"
- Busca automática de coordenadas via endereço
- Upload de imagem
- Salvamento via REST

### Mapa Interativo
- Exibição de todos os usuários
- Cor do marcador baseada no Rank
- Arrastar marcador para alterar localização
- Reverse Geocoding automático
- Atualização dinâmica via REST API

### Lista Administrativa
- Filtros por:
  - Nome
  - Rank
  - Setor
  - Status
- Edição rápida
- Exclusão (lixeira ou definitivo)
- Paginação local

### Painel Administrativo
- Navegação central do sistema
- Controle de sessão
- Separação de permissões (Funcionário x Admin)

---

## Requisitos para Funcionamento

Este projeto depende da estrutura correta do WordPress.

Não funciona apenas copiando os arquivos.

---

### 1. Custom Post Type

Slug obrigatório:

```
pessoas
```

Configuração necessária:
- show_in_rest = true
- suporte a título

---

### 2. Taxonomia

Slug obrigatório:

```
classificacao
```

Termos obrigatórios:
- em-analise
- latao
- bronze
- prata
- ouro

O slug precisa ser exatamente igual para que filtros e cores do mapa funcionem corretamente.

---

### 3. Campos ACF

Grupo vinculado ao CPT `pessoas`.

Campos obrigatórios:

- telefone (texto) — REQUIRED
- setor (texto ou select)
- status (texto ou select)
- latitude (texto)
- longitude (texto)
- endereco_texto (texto ou textarea)
- foto (imagem)
- rank (opcional se estiver usando taxonomia)

Observação:
O campo telefone está marcado como REQUIRED. Se estiver vazio, a REST API retorna erro 400.

---

### 4. Exposição dos Metadados na REST API

É necessário registrar os campos no functions.php:

```php
register_post_meta('pessoas', 'latitude', ['show_in_rest' => true]);
register_post_meta('pessoas', 'longitude', ['show_in_rest' => true]);
```

Ou utilizar register_rest_field.

---

### 5. Shortcode Obrigatório

As páginas precisam conter o shortcode:

```
[rest_cfg]
```

Ele fornece:
- Base da REST API
- Nonce de autenticação

Sem isso, o sistema não consegue salvar dados.

---

## Lógica de Atualização de Endereço

Existem duas formas de alterar localização:

### Digitando Endereço
- Geocoding via Nominatim
- Atualiza latitude e longitude
- Move o marcador
- Salva no ACF

### Arrastando Marcador
- Reverse Geocoding
- Endereço calculado substitui endereco_texto
- Atualiza coordenadas

---

## Permissões

- Funcionário: pode apenas criar cadastro
- Administrador: gerencia mapa, lista e classificação

---

## Observações Técnicas

- Sistema depende da estrutura correta do WordPress
- Slugs devem ser exatamente iguais
- Leaflet é carregado via CDN
- OpenStreetMap é utilizado como provedor de mapas
- Estrutura baseada em integração REST + ACF

---

## Licença

Projeto desenvolvido para portfólio e fins educacionais.
