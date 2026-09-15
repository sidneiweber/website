---
layout: post
title: 'Docker: qual a diferença entre ADD e COPY (e quando usar cada um)'
description: 'ADD e COPY parecem fazer a mesma coisa em um Dockerfile, e na maioria dos casos fazem. A diferença está nas funcionalidades extras do ADD: URLs, repositórios Git e extração de tar. Vamos entender o que cada uma faz e quando cada uma realmente faz sentido.'
date: '2026-09-14 21:00:00'
tags: [docker, dockerfile, containers, devops]
url: /diferenca-entre-docker-add-e-copy/
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
author: Sidnei Weber
---

Se você já escreveu um `Dockerfile`, provavelmente já se deparou com estas duas instruções:

```dockerfile
COPY app.py /app/
```

e:

```dockerfile
ADD app.py /app/
```

À primeira vista, parecem fazer exatamente a mesma coisa. E, na maioria dos casos simples, realmente fazem. Mas existe uma diferença importante:

> **`COPY` foi feito para copiar. `ADD` foi feito para adicionar — e pode fazer algumas coisas a mais no caminho.**

Neste guia, vamos visualizar essas diferenças e entender **quando usar cada uma**.

---

## 1. Antes de tudo: o build context

Imagine que temos este projeto:

```text
meu-projeto/
├── Dockerfile
├── app.py
├── requirements.txt
├── static/
│   └── style.css
└── templates/
    └── index.html
```

E executamos:

```bash
docker build -t meu-projeto .
```

Esse `.` não está ali por acaso: ele define o **build context**, que o Docker utiliza como a fonte dos arquivos disponíveis durante o build. Por isso, algo como:

```dockerfile
COPY app.py /app/
```

significa, essencialmente:

> Pegue `app.py` do build context e coloque-o em `/app/` dentro da imagem.

---

## 2. COPY: faça exatamente o que o nome diz

O `COPY` é a instrução mais simples de entender. Ele copia um arquivo:

```dockerfile
COPY app.py /app/
```

Também podemos copiar diretórios:

```dockerfile
COPY static/ /app/static/
```

Ou vários arquivos de uma vez:

```dockerfile
COPY requirements.txt app.py /app/
```

A ideia é simples:

```text
             COPY

┌──────────────────┐
│ Build Context    │
│                  │
│  app.py          │
│  static/         │
│  templates/      │
└────────┬─────────┘
         │
         │ copia
         ▼
┌──────────────────┐
│ Docker Image     │
│                  │
│  /app/           │
│    app.py        │
│    static/       │
└──────────────────┘
```

Além do build context, o `COPY` também consegue copiar de outras fontes usando `COPY --from`. A mais comum é **outra etapa de um multi-stage build**:

```dockerfile
FROM node AS build

WORKDIR /app
COPY . .
RUN npm run build


FROM nginx

COPY --from=build /app/dist /usr/share/nginx/html
```

Nesse caso, o `COPY` não está copiando diretamente do seu computador para a imagem final — ele está copiando arquivos produzidos pela etapa `build`.

O `--from` também aceita uma **imagem externa**, o que é útil para pegar um único arquivo de uma imagem pronta sem precisar construí-la:

```dockerfile
COPY --from=nginx:latest /etc/nginx/nginx.conf /tmp/nginx.conf
```

---

## 3. Então por que o ADD existe?

Aqui começa a parte interessante. O `ADD` também consegue fazer o trabalho básico de copiar arquivos:

```dockerfile
ADD app.py /app/
```

Até aqui:

```text
COPY app.py /app/
ADD  app.py /app/
```

produzem essencialmente o mesmo resultado. A diferença é que o `ADD` possui funcionalidades adicionais. É justamente aí que mora a questão.

---

## 4. ADD sabe buscar coisas de fora

O `ADD` pode buscar um recurso remoto durante o build:

```dockerfile
ADD https://example.com/arquivo.tar.gz /tmp/
```

O `COPY` não possui funcionalidade equivalente para simplesmente apontar uma URL como origem. Visualmente:

```text
COPY

arquivo local
     │
     ▼
┌─────────────┐
│ Docker      │
│ Image       │
└─────────────┘


ADD

🌐 Internet
     │
     │ download
     ▼
┌─────────────┐
│ Docker      │
│ Image       │
└─────────────┘
```

Ele também aceita a URL de um **repositório Git**, clonando o conteúdo direto para dentro da imagem:

```dockerfile
ADD https://github.com/usuario/repo.git#main /app/
```

E, em builds com BuildKit, existe um detalhe que torna o download durante o build bem mais defensável: a flag `--checksum`, que valida o arquivo baixado antes de colocá-lo na imagem.

```dockerfile
ADD --checksum=sha256:0000000000000000000000000000000000000000000000000000000000000000 \
    https://example.com/arquivo.tar.gz /tmp/
```

Sem isso, você está confiando que o conteúdo daquela URL nunca vai mudar — o que nem sempre é verdade.

Tudo isso torna o `ADD` mais poderoso. Mas também significa que existe **mais comportamento para você entender e controlar**.

---

## 5. A característica mais famosa do ADD: extração de tar

Essa é provavelmente a diferença que mais aparece quando alguém explica `ADD` vs `COPY`. Imagine que temos um `app.tar.gz` no build context. Com `COPY`:

```dockerfile
COPY app.tar.gz /app/
```

O resultado é:

```text
/app/app.tar.gz
```

O arquivo continua sendo um arquivo compactado. Já com `ADD`:

```dockerfile
ADD app.tar.gz /app/
```

o tar local é automaticamente extraído, e o resultado passa a ser:

```text
/app/
├── index.html
├── app.py
├── static/
└── templates/
```

Ou seja:

```text
                 ADD

              app.tar.gz
                  📦
                   │
                   │
                   ▼
             ┌───────────┐
             │    ADD    │
             │           │
             │  extrai   │
             └─────┬─────┘
                   │
          ┌────────┼────────┐
          ▼        ▼        ▼
       app.py   static/  templates/
```

Essa funcionalidade é bastante útil em situações específicas, mas também é uma das razões pelas quais o `ADD` pode ser menos óbvio para quem está lendo um `Dockerfile`.

---

## 6. A pegadinha que derruba muita gente

Repare que eu escrevi "tar **local**" na seção anterior. Isso não foi por acaso, e é aqui que mora a maior confusão sobre o `ADD`:

> **`ADD` extrai arquivos compactados que vêm do build context, mas NÃO extrai arquivos compactados que vêm de uma URL.**

Então isto extrai:

```dockerfile
ADD app.tar.gz /app/
```

E isto **não** extrai:

```dockerfile
ADD https://example.com/app.tar.gz /app/
```

No segundo caso, você termina com `/app/app.tar.gz` ainda compactado dentro da imagem. É comum alguém juntar as duas capacidades do `ADD` na cabeça e esperar que ele baixe e descompacte de uma vez — não é o que acontece.

Existe uma segunda pegadinha, mais sutil, que é de legibilidade. Imagine que alguém encontre este trecho:

```dockerfile
ADD app.tar.gz /app/
```

Para entender exatamente o que está acontecendo, essa pessoa precisa saber que `ADD` está sendo usado, que o arquivo é um tar, que arquivos tar locais são automaticamente extraídos e, portanto, que o conteúdo será colocado em `/app/`. Agora compare com:

```dockerfile
COPY app.tar.gz /app/
```

Aqui não existe interpretação. É simplesmente: copie este arquivo para lá. É justamente por isso que a documentação e as boas práticas do Docker recomendam **preferir `COPY` quando você só precisa copiar arquivos**.

---

## 7. Mas ADD é considerado "ruim"?

Não, e esse é um dos maiores mitos sobre o assunto. `ADD` não é um comando proibido nem inseguro por natureza — ele existe justamente porque possui funcionalidades que `COPY` não possui. A questão é outra:

> **Você precisa dessas funcionalidades?**

Se a resposta for não, `COPY` é a escolha mais clara. Se você realmente precisa de uma funcionalidade específica do `ADD`, então utilizá-lo faz todo sentido.

---

## 8. E o build cache?

Aqui existe outro detalhe importante: a escolha entre `COPY` e `ADD` não deve ser analisada isoladamente. A ordem das instruções no `Dockerfile` também influencia bastante o aproveitamento do cache do build. Considere:

```dockerfile
COPY requirements.txt /app/

RUN pip install -r /app/requirements.txt

COPY . /app/
```

Essa organização é interessante porque alterações no código da aplicação não invalidam a camada que instala as dependências. Se fizéssemos o contrário:

```dockerfile
COPY . /app/

RUN pip install -r /app/requirements.txt
```

qualquer alteração dentro do contexto copiado faria o Docker refazer a instalação das dependências, mesmo que o `requirements.txt` não tenha mudado uma vírgula. O próprio Docker recomenda organizar as instruções pensando na reutilização do cache. Portanto:

> **Não basta saber usar `COPY` e `ADD`. É importante saber o que você está copiando e em qual momento do build.**

---

## 9. E o `.dockerignore`?

Outro personagem importante nessa história é o `.dockerignore`. Imagine que seu projeto tenha:

```text
meu-projeto/
├── Dockerfile
├── app.py
├── node_modules/
├── .git/
├── logs/
├── backups/
└── arquivos-temporarios/
```

Você provavelmente não quer mandar tudo isso para o build context. Um `.dockerignore` resolve:

```text
.git
node_modules
logs
backups
*.tmp
```

Assim, o contexto enviado ao builder fica menor e mais controlado — o build fica mais rápido e você evita que arquivos desnecessários (ou sensíveis) acabem entrando na imagem por causa de um `COPY . .` distraído.

---

## 10. Afinal, qual devo usar?

Aqui está o resumo que eu gostaria de ter encontrado quando comecei a usar Docker:

| Situação                                                 | Use           |
| -------------------------------------------------------- | ------------- |
| Copiar um arquivo                                        | `COPY`        |
| Copiar um diretório                                      | `COPY`        |
| Copiar código da aplicação                               | `COPY`        |
| Copiar arquivos entre stages ou de outra imagem          | `COPY --from` |
| Copiar arquivos de forma explícita e previsível          | `COPY`        |
| Baixar um recurso remoto durante o build                 | `ADD`         |
| Clonar um repositório Git durante o build                | `ADD`         |
| Extrair automaticamente um tar local                     | `ADD`         |
| Não sabe qual usar                                       | **COPY**      |

Em formato de decisão:

```text
             Precisa colocar um arquivo
             ou diretório na imagem?
                       │
                       ▼
                    COPY
                       │
                       │
            Precisa de alguma
            funcionalidade especial
                 do ADD?
                 /       \
               NÃO       SIM
                │         │
                ▼         ▼
              COPY       ADD
```

Ou, de forma ainda mais simples:

> **Comece com `COPY`. Use `ADD` quando você tiver um motivo específico para usar `ADD`.**

---

## 11. Um exemplo real com COPY

Um `Dockerfile` comum para uma aplicação Python poderia ser assim:

```dockerfile
FROM python:3.13-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

Não precisamos de `ADD` aqui. Estamos apenas copiando o `requirements.txt`, instalando as dependências e copiando o código da aplicação — nessa ordem, justamente para aproveitar o cache como vimos na seção 8. Então `COPY` descreve perfeitamente a intenção.

---

## 12. E quando ADD realmente faz sentido?

Um caso clássico é instalar um binário de uma release oficial, validando o que foi baixado:

```dockerfile
FROM alpine:3.20

ARG KUBECTL_VERSION=v1.31.0

# Substitua pelo sha256 publicado junto com a release
ADD --checksum=sha256:0000000000000000000000000000000000000000000000000000000000000000 \
    https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl \
    /usr/local/bin/kubectl

RUN chmod +x /usr/local/bin/kubectl
```

Aqui o `ADD` não é uma alternativa preguiçosa ao `COPY`: ele está fazendo algo que o `COPY` simplesmente não faz. Como curiosidade, tanto `ADD` quanto `COPY` aceitam `--chown` e `--chmod` no BuildKit, então aquele `RUN chmod` até poderia virar `--chmod=755` na própria instrução.

Outro caso é receber um artefato de build já empacotado e querer o conteúdo extraído dentro da imagem:

```dockerfile
FROM nginx:alpine

ADD site.tar.gz /usr/share/nginx/html/
```

Uma linha, e o conteúdo do tar já está no lugar certo.

O importante é que, ao olhar para o Dockerfile daqui a alguns meses, você consiga responder:

> **"Por que aqui está ADD e não COPY?"**

Se a resposta for clara, provavelmente você está usando a instrução certa.

---

## Conclusão

`ADD` e `COPY` não são comandos completamente diferentes. Ambos colocam arquivos e diretórios dentro de uma imagem Docker, e para a grande maioria do que fazemos no dia a dia — copiar código, copiar dependências, copiar configuração — os dois entregam o mesmo resultado.

A diferença está nas funcionalidades extras do `ADD`: buscar recursos de URLs, clonar repositórios Git e extrair arquivos tar locais. São recursos legítimos e úteis, mas que exigem que quem lê o `Dockerfile` saiba de cor como eles funcionam. E, como vimos, nem sempre eles se combinam da forma que a gente imagina.

Então se for para levar uma única coisa deste artigo: **comece com `COPY` e só troque por `ADD` quando você souber dizer por quê**. Seu eu do futuro, revisando esse Dockerfile às duas da manhã, vai agradecer.
