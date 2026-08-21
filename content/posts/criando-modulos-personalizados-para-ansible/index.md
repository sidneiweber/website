---
layout: post
title: Criando seu primeiro módulo personalizado para Ansible
description: 'A coleção de módulos que vem com o Ansible cobre quase toda a rotina, mas cedo ou tarde aparece aquela necessidade específica que nenhum módulo atende. Veja como escrever, documentar e testar o seu próprio módulo em Python.'
date: '2026-08-20 21:00:00'
tags: [ansible, python, devops, automação]
url: /criando-modulos-personalizados-para-ansible/
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpeg"
author: Sidnei Weber
---
<!--more-->

Ansible é uma ferramenta de automação conhecida por sua abordagem *agentless*, ou seja, não é preciso instalar nada nos nós gerenciados, e pela simplicidade dos playbooks escritos em YAML. Por baixo dos panos, ele se conecta aos hosts e executa pequenos programas chamados **módulos**.

A coleção de módulos que já vem junto cobre praticamente tudo que é rotina: instalar pacotes, gerenciar serviços, copiar arquivos, criar usuários. Mas cedo ou tarde aparece aquela necessidade muito específica que nenhum módulo existente atende. É aí que escrever o seu próprio módulo passa a valer a pena.

## Por que criar um módulo personalizado

Alguns cenários típicos:

* Você precisa conversar com uma API interna ou um sistema proprietário que não tem módulo correspondente.
* Você quer abstrair uma sequência complexa de comandos — ou uma regra específica do seu ambiente em uma única ação reutilizável.
* Você quer garantir **idempotência**: rodar a operação uma ou dez vezes deve levar ao mesmo estado final, sem efeitos colaterais depois que o estado desejado foi atingido.

Existe ainda um benefício menos óbvio: escrever um módulo te obriga a entender como o Ansible funciona por dentro, por exemplo, como os argumentos chegam, como o resultado volta e o que o `changed` realmente significa.

## Preparando o ambiente

Um módulo é, na prática, um script autônomo que executa uma ação e devolve JSON. Pode ser escrito em qualquer linguagem, mas a maioria é em Python (o próprio Ansible é Python). Para alvos Windows, a linguagem primária é PowerShell.

Comece com uma estrutura mínima de diretórios:

```bash
mkdir module_example
cd module_example
mkdir library
```

O diretório `library`, ao lado do playbook, é um dos lugares onde o Ansible procura módulos personalizados automaticamente.

## Anatomia de um módulo em Python

Todo módulo Ansible faz três coisas:

1. Recebe argumentos e os valida.
2. Executa a ação (respeitando o `check_mode`, se suportado).
3. Retorna um JSON no `stdout` — nunca use `print()` para isso.

A classe `AnsibleModule`, de `ansible.module_utils.basic`, cuida das três partes. Além dela, os módulos declaram variáveis de documentação — `DOCUMENTATION`, `EXAMPLES` e `RETURN` — que alimentam o comando `ansible-doc`.

> Você vai encontrar `ANSIBLE_METADATA` em muito material mais antigo, mas ele foi descontinuado a partir do Ansible 2.10. Em módulos novos pode simplesmente omitir.

## Exemplo: um módulo hello

Crie o arquivo `library/hello.py`:

```python
#!/usr/bin/python

DOCUMENTATION = '''
---
module: hello
short_description: Um módulo que diz olá
version_added: "2.8"
description:
  - Um módulo que diz olá para uma pessoa especificada.
options:
  name:
    description:
      - Nome da pessoa a ser cumprimentada.
    required: false
    type: str
    default: John Doe
author:
  - Sidnei Weber
'''

EXAMPLES = '''
# Passando um nome personalizado
- name: Dizer olá para Linus Torvalds
  hello:
    name: "Linus Torvalds"
'''

RETURN = '''
fact:
  description: String de saudação
  type: str
  returned: always
  sample: "Olá John Doe!"
'''

from ansible.module_utils.basic import AnsibleModule

SAUDACAO = "Olá {name}!"


def run_module():
    # Argumentos aceitos pelo módulo
    module_args = dict(
        name=dict(type='str', required=False, default='John Doe'),
    )

    # supports_check_mode=True permite rodar com --check
    module = AnsibleModule(
        argument_spec=module_args,
        supports_check_mode=True,
    )

    result = dict(
        changed=False,
        fact='',
    )

    result['fact'] = SAUDACAO.format(name=module.params['name'])

    # Em check mode, retorne antes de qualquer alteração real
    if module.check_mode:
        module.exit_json(**result)

    # Um módulo que realmente altera algo definiria result['changed'] = True
    # apenas quando o estado do host mudou de fato.
    module.exit_json(**result)


def main():
    run_module()


if __name__ == '__main__':
    main()
```

### O que cada parte faz

| Trecho | Papel |
| --- | --- |
| `#!/usr/bin/python` | Define o interpretador do script. Use exatamente esta forma: o Ansible reescreve essa linha com o Python que descobrir no host (`ansible_python_interpreter`). Um `#!/usr/bin/env python` não é reconhecido e é usado literalmente — o que quebra em distros que só têm `python3` no `PATH`. |
| `DOCUMENTATION` / `EXAMPLES` / `RETURN` | Documentação exposta pelo `ansible-doc`. Não são opcionais se você pretende compartilhar o módulo. |
| `argument_spec` | Declara os parâmetros, com `type`, `required` e `default`. O Ansible valida antes do seu código rodar. |
| `supports_check_mode=True` | Sinaliza que o módulo sabe se comportar em *dry run*. |
| `result['changed']` | Indica se o host foi alterado. É o que faz o Ansible reportar `changed=1` no recap — a base da idempotência. |
| `module.exit_json(**result)` | Serializa o resultado como JSON e encerra o módulo com sucesso. |

Para erros, o par correspondente é `module.fail_json(msg="...")` — sempre com uma mensagem explicando o que deu errado.

Um detalhe fácil de escorregar: o `default` do `argument_spec` e o `default` declarado no `DOCUMENTATION` precisam ser o mesmo valor. Divergência aqui gera documentação mentirosa e é um dos erros mais comuns em módulos caseiros.

## Usando o módulo em um playbook

No diretório `module_example`, crie o arquivo `playbook.yaml`:

```yaml
---
- name: Usando módulo customizado
  hosts: all
  tasks:
    - name: Executa o módulo customizado hello
      hello:
        name: "Alexandra"
      register: demo_greeting

    - name: Imprime a saída do módulo
      debug:
        msg: "{{ demo_greeting.fact }}"
```

O `register` captura a saída JSON do módulo numa variável e `demo_greeting.fact` acessa diretamente o campo que definimos no `result`.

## Executando

Você vai precisar de um inventário. Para testar localmente, crie um arquivo `inventory`:

```ini
[all]
localhost ansible_connection=local
```

Ou, para um host remoto:

```ini
[all]
192.0.2.10 ansible_user=seu_usuario
```

Como o `library/` está ao lado do playbook, o Ansible já encontra o módulo. Se ele estiver em outro lugar, aponte com a variável de ambiente `ANSIBLE_LIBRARY`:

```bash
ansible-playbook -i inventory playbook.yaml

# ou, com o library em outro caminho:
ANSIBLE_LIBRARY=./library ansible-playbook -i inventory playbook.yaml
```

Saída esperada:

```text
TASK [Gathering Facts] *********************************************
ok: [localhost]

TASK [Executa o módulo customizado hello] **************************
ok: [localhost]

TASK [Imprime a saída do módulo] ***********************************
ok: [localhost] => {
    "msg": "Olá Alexandra!"
}

PLAY RECAP *********************************************************
localhost : ok=3  changed=0  unreachable=0  failed=0  skipped=0
```

## Testando o módulo isoladamente

Antes de colocar num playbook, dá para executar o módulo direto, passando os argumentos num JSON. É o jeito mais rápido de iterar:

```bash
echo '{"ANSIBLE_MODULE_ARGS": {"name": "Alexandra"}}' | python3 library/hello.py
```

```json
{"changed": false, "fact": "Ol\u00e1 Alexandra!", "invocation": {"module_args": {"name": "Alexandra"}}}
```

O `Ol\u00e1` é só o escape padrão de acentos no JSON, dentro do playbook o Ansible desserializa e exibe `Olá` normalmente.

E para conferir se a documentação está bem formada:

```bash
ansible-doc -M ./library hello
```

Se o YAML dentro do `DOCUMENTATION` estiver quebrado, esse comando falha, o que é justamente o teste que você quer.

## Compartilhando e reutilizando

Um módulo solto no `library/` resolve o problema local. Para distribuir, existem dois formatos:

* **Roles**: estrutura padronizada para agrupar tasks, variáveis, templates, handlers e módulos. São parametrizáveis e ótimas para reutilizar lógica dentro da sua organização.
* **Collections**: o formato moderno de distribuição de conteúdo Ansible: playbooks, roles, módulos, plugins e documentação empacotados juntos, versionados e publicáveis no Galaxy ou num repositório privado.

O comando `ansible-galaxy` faz os dois lados: baixa conteúdo da comunidade e inicializa o esqueleto do seu próprio.

```bash
ansible-galaxy collection init minha_empresa.utilitarios
```

Dentro da collection os módulos vivem em `plugins/modules/` e passam a ser chamados pelo nome completo `minha_empresa.utilitarios.hello`.

## Conclusão

Escrever módulos personalizados é o passo que separa "usar o Ansible" de "estender o Ansible". Vale lembrar dos dois pontos que fazem um módulo ser bom em vez de apenas funcional: **idempotência**, reportar `changed` só quando algo realmente mudou e **documentação**, porque o próximo a ler seu módulo provavelmente é você, seis meses depois.

## Referências

* [Developing modules — Ansible Documentation](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html)
* [Ansible module architecture](https://docs.ansible.com/ansible/latest/dev_guide/developing_program_flow_modules.html)
* [Developing collections](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html)
