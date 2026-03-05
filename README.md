# RUNBOOK

## Pipeline CI/CD para Container Image (Python + GitHub Actions)

Projeto: python-image\
Objetivo: Automatizar build, teste e publicação de uma imagem Docker.

---

# 1. Visão Geral

Este projeto implementa uma pipeline CI/CD para construção e publicação
de uma imagem Docker utilizando:

- Python (Flask)
- Docker
- GitHub Actions
- SonarCloud
- Docker Hub

Fluxo da pipeline:

Developer → CI Pipeline → Pull Request → Merge → Build Image → Push
Docker Hub

---

# 2. Arquitetura da Pipeline

    Feature Branch
         │
         ▼
    GitHub Actions (CI)
         │
         ├── Lint (pylint)
         ├── Testes (pytest)
         └── Artefatos
         │
         ▼
    Pull Request automático
         │
         ▼
    Merge na branch main
         │
         ▼
    Pipeline Publish
         │
         ├── SonarCloud Scan
         ├── Docker Build
         └── Docker Push

---

# 3. Estrutura do Projeto

    python-image
    │
    ├── .github/workflows
    │   ├── 0-feature.yml
    │   ├── 1-main.yml
    │   ├── python-ci.yml
    │   └── publish.yml
    │
    ├── src
    │   ├── app.py
    │   ├── requirements.txt
    │   └── test_app.py
    │
    ├── Dockerfile
    ├── settings.yml
    ├── sonar-project.properties
    └── README.md

---

# 4. Execução local

## Clonar repositório

```bash
git clone git@github.com:wekers/python-image.git
cd python-image
```

---

## Instalar dependências

    pip install --no-cache -r src/requirements.txt

---

## Executar aplicação

    python3 src/app.py

Aplicação exposta em:

    http://localhost:9000

---

## Testar API

    curl http://localhost:9000

Resposta:

    Seja bem-vindo!

Criar time:

    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{ "nome": "Corinthians", "pais": "Brasil" }' \
    http://localhost:9000/times

---

# 5. Testes automatizados

Executar testes:

    pytest src

Testes cobrem:

- endpoint principal
- criação de times
- criação de campeonatos
- tratamento de erro 404

---

# 6. Build da imagem Docker

Construir imagem:

    docker build -t python-image:v1.0.0 .

Executar container:

    docker run -p 9000:9000 -d python-image:v1.0.0

---

# 7. Pipeline CI (Feature Branch)

Arquivo:

    .github/workflows/0-feature.yml

Executado quando ocorre push em:

    feature/**
    fix/**

Executa workflow:

    python-ci.yml

Etapas:

- checkout
- setup python
- install dependencies
- lint (pylint)
- testes (pytest)
- upload artefatos
- criação automática de Pull Request

---

# 8. Pipeline de Publicação

Arquivo:

    .github/workflows/1-main.yml

Executado quando:

    Pull Request é mergeado na main

Executa workflow:

    publish.yml

Etapas:

1.  checkout
2.  sonarcloud scan
3.  login docker hub
4.  docker build
5.  docker push

---

# 9. Tag da imagem

A tag é gerada automaticamente a partir do SHA do commit:

    ${GITHUB_SHA:0:7}

Exemplo:

    wekers/python-image:3fa4b7a

Benefícios:

- versionamento imutável
- rastreabilidade
- auditoria

---

# 10. Docker Hub

Imagem publicada em:

    https://hub.docker.com/r/wekers/python-image

---

# 11. SonarCloud

Análise de qualidade de código inclui:

- bugs
- code smells
- vulnerabilidades
- análise estática

Configuração:

    sonar-project.properties

---

# 12. Melhorias Futuras

Possíveis evoluções da pipeline:

- adicionar build multi-arch
- scan de vulnerabilidades (Trivy)
- cobertura de testes
- deploy automático em Kubernetes
- cache de dependências
- buildkit docker

---

# 13. Conclusão

Esta pipeline implementa um fluxo DevOps completo para:

- integração contínua
- build automatizado
- qualidade de código
- publicação de imagens container

Utilizando ferramentas amplamente adotadas no mercado.
