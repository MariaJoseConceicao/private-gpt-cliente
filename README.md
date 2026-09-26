# Private GPT — Guia de instalação

Este pacote permite correr o Private GPT localmente no teu computador,
para fazeres perguntas sobre os teus próprios documentos, sem enviar
nada para a internet.

## Requisitos

- **Docker Desktop** instalado e a correr.
  Download: https://www.docker.com/products/docker-desktop/
- Pelo menos **8 GB de RAM livres** (o modelo de IA corre localmente e
  consome memória).
- Ligação à internet apenas na primeira vez, para descarregar as
  imagens e os modelos (~5 GB no total). Depois disso funciona
  totalmente offline.

## Passo 1 — Clonar este repositório

Abre um terminal (PowerShell, CMD ou terminal do Mac/Linux) e corre:

```bash
git clone <URL_DESTE_REPOSITORIO>
cd <NOME_DA_PASTA>
```

## Passo 2 — Arrancar os containers

Na pasta do projeto, corre:

```bash
docker compose up -d
```

Isto vai:
1. Descarregar as imagens necessárias (só na primeira vez).
2. Arrancar o motor de IA (Ollama) e a aplicação Private GPT.
3. Descarregar automaticamente os modelos de IA necessários
   (`llama3.1` e `nomic-embed-text`, ~5 GB no total).

## Passo 3 — Esperar pelo download dos modelos (primeira vez)

O download dos modelos demora alguns minutos, dependendo da tua
internet. Para acompanhar o progresso, corre:

```bash
docker compose logs -f private-gpt
```

Quando vires esta linha, está pronto:

```
Uvicorn running on http://0.0.0.0:8001
```

Podes fechar os logs com `Ctrl+C` (isto não pára os containers).

## Passo 4 — Abrir a aplicação

Abre o browser em:

```
http://localhost:8001
```

Já podes fazer upload dos teus documentos (botão "Upload File(s)") e
fazer perguntas sobre o conteúdo deles, no modo **RAG**.

## Comandos úteis

| Ação | Comando |
|---|---|
| Parar a aplicação | `docker compose down` |
| Voltar a arrancar | `docker compose up -d` |
| Ver logs em direto | `docker compose logs -f private-gpt` |
| Apagar tudo (incluindo modelos descarregados) | `docker compose down -v` |

## Perguntas frequentes

**A primeira resposta demora muito tempo.**
É normal — o modelo corre no CPU. Respostas seguintes tendem a ser
mais rápidas. Se tiveres uma placa gráfica NVIDIA, podes ativar o
modo GPU descomentando as linhas correspondentes no
`docker-compose.yaml`.

**Quero mudar de modelo de IA.**
Edita as linhas `PGPT_OLLAMA_LLM_MODEL` e `PGPT_OLLAMA_EMBEDDING_MODEL`
no `docker-compose.yaml` com o nome do modelo Ollama que preferires,
e depois corre `docker compose up -d --force-recreate private-gpt`.

**Os meus documentos ficam guardados onde?**
Ficam guardados num volume Docker interno (`private_gpt_data`), que
persiste entre reinícios. Não são enviados para a internet.
