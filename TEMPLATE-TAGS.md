# Como editar os templates `clinica.docx` e `face.docx`

Estes dois `.docx` são os **modelos** do contrato. Você pode abrir no Word/Google Docs e
**editar livremente o texto, a formatação, as cláusulas e o visual** — desde que **não altere
as tags entre chaves duplas** `{{ ... }}`. São elas que o sistema preenche com os dados do site.

## Regra de ouro
- **Pode** mudar texto, fontes, cores, negrito, margens, cabeçalho/rodapé, ordem das cláusulas.
- **Não** apague nem altere nada dentro de `{{ }}`.
- Todo bloco aberto precisa do fechamento (ver loops abaixo).
- Para mover um campo, recorte e cole a tag inteira (com as chaves).

## Campos do contrato (uma vez)
| Tag | Vira |
|-----|------|
| `{{inicioContrato}}` | Data por extenso (ex.: 14 de junho de 2026) |
| `{{vencimento}}` | Dia de vencimento |
| `{{valorAluguel}}` | Valor mensal |
| `{{prazoContrato}}` | Permanência mínima (ex.: 12 (doze) meses) |

## Locatários — bloco que repete por inquilino
Tudo entre `{{#locatarios}}` e `{{/locatarios}}` se repete para cada inquilino.
| Tag | Vira |
|-----|------|
| `{{numero}}` | 1, 2, 3… |
| `{{qualificacao}}` | A frase completa de qualificação, **já no gênero certo** (ex.: "Fulana, brasileira, RG ou CRO…, inscrita no CPF…, residente e domiciliada à…"). Montada pelo sistema a partir dos dados + **Sexo**. |
| `{{nome}}` | Nome (usado na assinatura e na solicitação) |
| `{{rg}}` `{{cpf}}` | RG/CRO e CPF (usados na solicitação) |
| `{{portador}}` | "portador"/"portadora" conforme o sexo |

> A **Solicitação de Integração ao Corpo Clínico** está dentro deste mesmo bloco — por isso sai
> **uma por inquilino**, já preenchida.

## Fiadores — bloco que repete por fiador
Entre `{{#fiadores}}` e `{{/fiadores}}`:
| Tag | Vira |
|-----|------|
| `{{numero}}` | 1, 2, 3… |
| `{{qualificacao}}` | Qualificação completa do fiador, no gênero certo (inclui estado civil) |
| `{{anuencia}}` | Frase da anuência do cônjuge (vazia se não houver) |
| `{{nome}}` | Nome (assinatura) |

### Cônjuge anuente (condicional)
Dentro do bloco de fiadores, o que estiver entre `{{#temConjuge}}` e `{{/temConjuge}}` **só
aparece se o fiador for casado/união e tiver cônjuge informado**:
```
{{#temConjuge}}
{{conjugeNome}} – CÔNJUGE ANUENTE DO FIADOR {{numero}}
{{/temConjuge}}
```

## Blocos (sempre em par)
| Abre | Fecha | Efeito |
|------|-------|--------|
| `{{#locatarios}}` | `{{/locatarios}}` | repete por locatário |
| `{{#fiadores}}` | `{{/fiadores}}` | repete por fiador |
| `{{#temConjuge}}` | `{{/temConjuge}}` | só com cônjuge anuente |

## Por que a qualificação é uma tag só (`{{qualificacao}}`)?
Para o texto sair **sem genéricos** ("brasileira/inscrita/portadora" no gênero certo, e omitindo
e-mail/telefone vazios), o sistema monta a frase de qualificação automaticamente a partir dos
campos + **Sexo**. Assim o modelo fica limpo e bonito. Se quiser mudar o **formato** dessa frase,
me peça — está no `index.html` (funções `qualLoc`/`qualFid`).

## Formatação
O modelo já vem com A4, margens, **cabeçalho e rodapé de página por clínica** (nome + CNPJ no topo,
endereço + nº de página no rodapé), título estilizado e acento dourado. Tudo isso é editável no Word.

## Se algo quebrar
Erro de preenchimento costuma ser tag incompleta (faltou `}}`) ou bloco sem fechamento. Há cópias
`.docx.bak*` na pasta para restaurar.
