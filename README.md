# artjason.com

Site institucional da JWL TECH — consultoria e soluções de TI.

Projeto desenvolvido com auxílio de inteligência artificial para fins de estudo e aprendizado em desenvolvimento web, containerização e pipelines CI/CD.

## Stack

- HTML, CSS e JS puros (sem frameworks)
- Nginx Alpine (servidor web)
- Docker (containerização)
- GitHub Actions (CI/CD)
- Kubernetes + ArgoCD (deploy via GitOps)

## Estrutura

```
├── index.html          # Home
├── servicos.html       # Serviços
├── stack.html          # Stack técnica
├── 404.html            # Página de erro
├── css/                # Estilos
├── js/                 # Scripts
├── img/                # Imagens
├── nginx.conf          # Configuração do Nginx
├── Dockerfile          # Build da imagem
└── DESIGN-SYSTEM.md    # Guia de padronização visual
```

## Deploy

O deploy é automatizado via tag:

```bash
git tag v1.0.0
git push origin v1.0.0
```

A GitHub Action builda a imagem Docker, pusha pro Docker Hub e abre um PR no repositório GitOps para atualizar o cluster.

## Aviso

Este projeto foi criado com auxílio de IA (Claude) para fins educacionais e de experimentação.
